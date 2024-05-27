o banco de dados tem que ter essa estrutura

![[Pasted image 20240514103554.png]]

é uma tabela many to many que relaciona o usuário com as permissões

modelo da classe permission

```java
package br.com.tizo.model;  
  
import jakarta.persistence.*;  
import org.springframework.security.core.GrantedAuthority;  
  
import java.io.Serializable;  
import java.util.Objects;  
  
@Entity  
@Table(name = "permission")  
public class Permission implements GrantedAuthority, Serializable {  
  
    private static final long serialVersionUID = 1L;  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
  
    @Column()  
    private String description;  
  
    public Permission() {}  
  
    public Long getId() {  
        return id;  
    }  
  
    public void setId(Long id) {  
        this.id = id;  
    }  
  
    public String getDescription() {  
        return description;  
    }  
  
    public void setDescription(String description) {  
        this.description = description;  
    }  
  
    @Override  
    public boolean equals(Object o) {  
        if (this == o) return true;  
        if (!(o instanceof Permission that)) return false;  
        return Objects.equals(id, that.id) && Objects.equals(description, that.description);  
    }  
  
    @Override  
    public int hashCode() {  
        return Objects.hash(id, description);  
    }  
  
    @Override  
    public String getAuthority() {  
        return this.description;  
    }  
}
```

modelo da classe users

```java
package br.com.tizo.model;  
  
import jakarta.persistence.*;  
import org.springframework.security.core.GrantedAuthority;  
import org.springframework.security.core.userdetails.UserDetails;  
  
import java.io.Serializable;  
import java.util.ArrayList;  
import java.util.Collection;  
import java.util.List;  
import java.util.Objects;  
  
@Entity  
@Table(name = "users")  
public class Users implements UserDetails, Serializable {  
  
    private static final long serialVersionUID = 1L;  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private Long id;  
  
    @Column(name = "user_name", unique = true)  
    private String userName;  
  
    @Column(name = "full_name")  
    private String fullName;  
  
    @Column(name = "password")  
    private String password;  
  
    @Column(name = "account_non_expired")  
    private Boolean accountNonExpired;  
  
    @Column(name = "account_non_locked")  
    private Boolean accountNonLocked;  
  
    @Column(name = "credentials_non_expired")  
    private Boolean credentialsNonExpired;  
  
    @Column(name = "enabled")  
    private Boolean enabled;  
  
    @ManyToMany(fetch = FetchType.EAGER)  
    @JoinTable(  
            name = "user_permission",  
            joinColumns = {@JoinColumn(name = "id_user")},  
            inverseJoinColumns = {@JoinColumn(name = "id_permission")})  
    private List<Permission> permissions;  
  
    public Users() {}  
  
    public List<String> getRoles(){  
        List<String> roles = new ArrayList<>();  
  
        for (Permission permission : permissions){  
            roles.add(permission.getDescription());  
        }  
        return roles;  
    }  
  
    @Override  
    public Collection<? extends GrantedAuthority> getAuthorities() {  
        return this.permissions;  
    }  
  
    @Override  
    public String getPassword() {  
        return this.password;  
    }  
  
    @Override  
    public String getUsername() {  
        return this.userName;  
    }  
  
    @Override  
    public boolean isAccountNonExpired() {  
        return this.accountNonExpired;  
    }  
  
    @Override  
    public boolean isAccountNonLocked() {  
        return this.accountNonLocked;  
    }  
  
    @Override  
    public boolean isCredentialsNonExpired() {  
        return this.credentialsNonExpired;  
    }  
  
    @Override  
    public boolean isEnabled() {  
        return this.enabled;  
    }  
  
    public Long getId() {  
        return id;  
    }  
  
    public void setId(Long id) {  
        this.id = id;  
    }  
  
    public String getUserName() {  
        return userName;  
    }  
  
    public void setUserName(String userName) {  
        this.userName = userName;  
    }  
  
    public String getFullName() {  
        return fullName;  
    }  
  
    public void setFullName(String fullName) {  
        this.fullName = fullName;  
    }  
  
    public void setPassword(String password) {  
        this.password = password;  
    }  
  
    public Boolean getAccountNonExpired() {  
        return accountNonExpired;  
    }  
  
    public void setAccountNonExpired(Boolean accountNonExpired) {  
        this.accountNonExpired = accountNonExpired;  
    }  
  
    public Boolean getAccountNonLocked() {  
        return accountNonLocked;  
    }  
  
    public void setAccountNonLocked(Boolean accountNonLocked) {  
        this.accountNonLocked = accountNonLocked;  
    }  
  
    public Boolean getCredentialsNonExpired() {  
        return credentialsNonExpired;  
    }  
  
    public void setCredentialsNonExpired(Boolean credentialsNonExpired) {  
        this.credentialsNonExpired = credentialsNonExpired;  
    }  
  
    public Boolean getEnabled() {  
        return enabled;  
    }  
  
    public void setEnabled(Boolean enabled) {  
        this.enabled = enabled;  
    }  
  
    public List<Permission> getPermissions() {  
        return permissions;  
    }  
  
    public void setPermissions(List<Permission> permissions) {  
        this.permissions = permissions;  
    }  
  
    @Override  
    public boolean equals(Object o) {  
        if (this == o) return true;  
        if (!(o instanceof Users users)) return false;  
        return Objects.equals(id, users.id) && Objects.equals(userName, users.userName) && Objects.equals(fullName, users.fullName) && Objects.equals(password, users.password) && Objects.equals(accountNonExpired, users.accountNonExpired) && Objects.equals(accountNonLocked, users.accountNonLocked) && Objects.equals(credentialsNonExpired, users.credentialsNonExpired) && Objects.equals(enabled, users.enabled) && Objects.equals(permissions, users.permissions);  
    }  
  
    @Override  
    public int hashCode() {  
        return Objects.hash(id, userName, fullName, password, accountNonExpired, accountNonLocked, credentialsNonExpired, enabled, permissions);  
    }  
}
```

o trecho abaixo da classe user configura a tabela "do meio" do relacionamento many to many

```java
@ManyToMany(fetch = FetchType.EAGER)  
    @JoinTable(  
            name = "user_permission",  
            joinColumns = {@JoinColumn(name = "id_user")},  
            inverseJoinColumns = {@JoinColumn(name = "id_permission")})  
    private List<Permission> permissions;  
```

repository e findByUserName

exemplo de método personalizado no JPA

```java
package br.com.tizo.repositories;  
  
import br.com.tizo.model.User;  
import org.springframework.data.jpa.repository.JpaRepository;  
import org.springframework.data.jpa.repository.Query;  
import org.springframework.data.repository.query.Param;  
  
public interface UserRepository extends JpaRepository<User, Long> {  
  
    @Query("SELECT u FROM User u WHERE u.userName =:userName")  
    User findByUsername(@Param("userName") String userName);  
}
```

service abaixo

```java
package br.com.tizo.services;  
  
import br.com.tizo.repositories.UserRepository;  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.security.core.userdetails.UserDetails;  
import org.springframework.security.core.userdetails.UserDetailsService;  
import org.springframework.security.core.userdetails.UsernameNotFoundException;  
import org.springframework.stereotype.Service;  
  
import java.util.logging.Logger;  
  
@Service  
public class UserServices implements UserDetailsService {  
  
    private Logger logger = Logger.getLogger(UserServices.class.getName());  
  
    @Autowired  
    UserRepository repository;  
  
    public UserServices(UserRepository repository) {  
       this.repository = repository;  
    }  
  
    @Override  
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {  
       logger.info("Finding one user by name " + username +"!");  
       var user = repository.findByUsername(username);  
  
       if(user != null){  
          return user;  
       }else {  
          throw new UsernameNotFoundException("" + username + " not found!");  
       }  
    }  
}
```


