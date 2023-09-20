# Relaciones de entidades en Springboot


```
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private int id;

    private String name;

    private String email;

    private String password;


    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```
Donde @GeneratedValue(strategy = GenerationType.AUTO) se usa para indicar que la variable será INT y AUTO_INCREMENT

# Relación 1 a Muchos
Finalmente las relaciones de tablas que necesitará usar son: 1 a muchos y muchos a muchos. Supongo que tiene una relación entre las entidades Curso y Profesor. Para configurar la relación usando JPA, las clases se verán así:

```
@Entity
@Table(name = "profesores")
public class Profesores {

    @Id
    @GeneratedValue(strategy= GenerationType.AUTO)
    private long id;

    private String name;

    @OneToMany(mappedBy = "profesor") //Nombre de la propiedad en la otra clase
    private List<Cursos> cursos;
    
    //No olvidar los Getters y Setters   
}

```

```
@Entity
@Table(name = "cursos")
public class Cursos {
    @Id
    @GeneratedValue(strategy= GenerationType.AUTO)
    private long id;

    private String name;

    private String program;

    @ManyToOne
    @JoinColumn(name = "profeID")
    Profesores profesor;

    @ManyToMany
    List<Estudiantes> estudiantes;

    //No olvidar los Getters y Setters
}
```



# Relación Muchos a Muchos
Suponga que tiene las entidades Estudiantes y Cursos. Para configurar la relación Muchos a Muchos entre estas dos tablas, necesita hacer lo siguiente:

```
@Entity
@Table(name = "estudiantes")
public class Estudiantes {
    @Id
    @GeneratedValue(strategy= GenerationType.AUTO)
    private long id;
    private String name;
    private String code;

    @ManyToMany
    @JoinTable(
            name = "estudiante_curso",
            joinColumns = @JoinColumn(name = "estudiante_id"),
            inverseJoinColumns = @JoinColumn(name = "curso_id")
    )
    private List<Cursos> cursos;

    //No olvidar Getters y Setters
    
}
```
Note que usted define la tabla pivote a través de @JoinTable

```
@Entity
@Table(name = "cursos")
public class Cursos {
    @Id
    @GeneratedValue(strategy= GenerationType.AUTO)
    private long id;

    private String name;

    private String program;

    @ManyToOne
    @JoinColumn(name = "profeID")
    Profesores profesores;

    @ManyToMany
    List<Estudiantes> estudiantes;

    //No olvidar Getters y Setters
    
}

```


