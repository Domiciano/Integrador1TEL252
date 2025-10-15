# Taller 4 · 19% de Implementación
Debe implementar el frontend de una API de tareas. Para esta oportunidad no tiene que gestionar las redirecciones de la página, debe hacer exclusivamente lo que se pide usando llenado de div por medio de `innerHTML` y solicitudes HTTP por medio de `fetch`.

`[25%]` 

Creación de tareas

Cree un formulario HTML que le permita obtener los datos necesarios para crear la tarea. Una vez tenga estos datos, cree un objeto a partir de ellos y cree la tarea haciendo la solicitud correcta al endpoint correcto.

`[25%]` 

Lista de tareas

Cree tres `div` vacíos que luego llene con la tareas por status. Sólo hay 3 posibles status: `todo`, `doing` y `done`.

Si lista todas las tareas en un sólo `div` tiene la mitad del punto

`[25%]` 

Cambiar status de la tarea

Cree un `form` que le permita obtener los datos necesarios para luego utilizarlos en la solicitud al endpoint que permite cambiar de estado una tarea.

`[25%]` 

Eliminación de tarea

Cree un `form` que le permita obtener los datos necesarios para luego utilizarlos en la solicitud al endpoint que permite eliminar una tarea

# Código del backend

El controlador del API es
```java
@RestController
@CrossOrigin
@RequestMapping("/tasks")
public class ExamenController {

    @Autowired
    private TaskRepo taskRepo;


    @GetMapping
    public ResponseEntity<?> getAllTasks() {
        return ResponseEntity.ok(taskRepo.findAll());
    }

    // status/todo
    // Para listar las tareas en status ToDO
    // status/doing
    // Para listar las tareas en status Doing
    // status/done
    // Para listar las tareas en status Done
    @GetMapping("/status/{status}")
    public ResponseEntity<?> getTasksByStatus(@PathVariable String status) {
        return ResponseEntity.ok(taskRepo.findByStatus(status));
    }

    @PostMapping
    public ResponseEntity<?> createTask(@RequestBody Task task) {
        return ResponseEntity.ok(taskRepo.save(task));
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<?> deleteTask(@PathVariable Long id) {
        taskRepo.deleteById(id);
        return ResponseEntity.ok().build();
    }

    // /3/doing cambia el estado de la tarea con id 3 a doing
    @PatchMapping("/{id}/{status}")
    public ResponseEntity<?> updateTaskStatus(@PathVariable Long id, @PathVariable String status) {
        Optional<Task> taskOptional = taskRepo.findById(id);
        if (taskOptional.isPresent()) {
            Task task = taskOptional.get();
            task.setStatus(status);
            return ResponseEntity.ok(taskRepo.save(task));
        }
        return ResponseEntity.notFound().build();
    }
}
```

Y la entidad de `Task` es
```java
@Entity
public class Task {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String description;
    private String status; //TODO, DOING Y DONE

    ...

}
```
