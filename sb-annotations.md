1. Core Spring Annotations

    - @Configuration – Defines a configuration class.
    - @Bean – Declares a Spring-managed bean.
    - @Component – Marks a class as a Spring component.
    - @Service – Specialization of @Component for service classes.
    - @Repository – Specialization of @Component for repository classes.
    - @ComponentScan – Enables component scanning.

2. Spring Boot Annotations

    - @SpringBootApplication – Combines @Configuration, @EnableAutoConfiguration, and @ComponentScan.
    - @EnableAutoConfiguration – Enables automatic configuration.
    - @ConditionalOnProperty – Enables a bean only if a specific property exists.
    - @ConditionalOnClass – Enables a bean only if a class is on the classpath.
   -  @ConditionalOnMissingBean – Enables a bean only if another bean is missing.

3. Dependency Injection & Bean Scope

    - @Autowired – Injects dependencies automatically.
    - @Qualifier – Specifies which bean to inject when multiple exist.
    - @Primary – Gives priority to a specific bean when multiple exist.
    - @Scope – Defines the scope of a bean.

4. Web & REST Annotations

    - @RestController – Combines @Controller and @ResponseBody.
    - @RequestMapping – Maps HTTP requests to handler methods.
    - @GetMapping, @PostMapping, @PutMapping, @DeleteMapping, @PatchMapping – Shortcut annotations for @RequestMapping.
    - @RequestParam – Extracts query parameters.
    - @PathVariable – Extracts variables from the URL path.
    - @RequestBody – Binds request body to a method parameter.
    - @ResponseBody – Indicates that the return value should be serialized as JSON.
    - @CrossOrigin – Enables CORS on a method or class.

5. Data & JPA Annotations

    - @Entity – Marks a class as a JPA entity.
    - @Table – Defines the table name for an entity.
    - @Id – Marks a field as the primary key.
    - @GeneratedValue – Specifies how the primary key is generated.
    - @Column – Configures a database column.
    - @OneToOne, @OneToMany, @ManyToOne, @ManyToMany – Define relationships between entities.
    - @Transactional – Marks a method or class as transactional.

6. Validation Annotations (JSR 303/380)

    - @Valid – Triggers validation on an object.
    - @NotNull, @NotEmpty, @NotBlank – Validate that a field is not null/empty.
    - @Size – Restricts string or collection length.
    - @Pattern – Ensures a string follows a regex pattern.
    - @Min, @Max – Define numeric constraints.

7. Caching & Scheduling

    - @Cacheable – Caches method results.
    - @CacheEvict – Removes cached entries.
    - @Scheduled – Defines scheduled tasks.
    - @Async – Runs a method asynchronously.

8. Security Annotations

    - @PreAuthorize – Specifies security conditions before a method runs.
    - @PostAuthorize – Specifies security conditions after a method runs.
    - @Secured – Restricts access to specific roles.
    - @RolesAllowed – Defines allowed roles.