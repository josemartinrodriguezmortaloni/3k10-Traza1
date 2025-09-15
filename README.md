# Ejercicio Traza 1 - Sistema de Gestión de Empresas y Sucursales

## 📋 Descripción del Ejercicio

Este proyecto implementa un sistema de gestión de empresas y sucursales utilizando Java, donde se modelan las relaciones entre diferentes entidades geográficas y empresariales. El ejercicio demuestra el uso de patrones de diseño como Builder Pattern, Repository Pattern y el manejo de relaciones bidireccionales entre entidades.

## 🏗️ Arquitectura del Sistema

### Entidades Principales

El sistema está compuesto por las siguientes entidades que mantienen relaciones jerárquicas:

#### 1. **Pais** (`src/entidades/Pais.java`)
- **Propósito**: Representa un país
- **Atributos**: 
  - `id`: Identificador único
  - `nombre`: Nombre del país
  - `provincias`: Conjunto de provincias que pertenecen al país

#### 2. **Provincia** (`src/entidades/Provincia.java`)
- **Propósito**: Representa una provincia dentro de un país
- **Atributos**:
  - `id`: Identificador único
  - `nombre`: Nombre de la provincia
  - `pais`: Referencia al país al que pertenece
  - `localidades`: Conjunto de localidades que pertenecen a la provincia

#### 3. **Localidad** (`src/entidades/Localidad.java`)
- **Propósito**: Representa una localidad dentro de una provincia
- **Atributos**:
  - `id`: Identificador único
  - `nombre`: Nombre de la localidad
  - `provincia`: Referencia a la provincia a la que pertenece

#### 4. **Domicilio** (`src/entidades/Domicilio.java`)
- **Propósito**: Representa una dirección física específica
- **Atributos**:
  - `id`: Identificador único
  - `calle`: Nombre de la calle
  - `numero`: Número de la dirección
  - `cp`: Código postal
  - `piso`: Piso (opcional)
  - `nroDpto`: Número de departamento (opcional)
  - `localidad`: Referencia a la localidad donde se encuentra

#### 5. **Empresa** (`src/entidades/Empresa.java`)
- **Propósito**: Representa una empresa con múltiples sucursales
- **Atributos**:
  - `id`: Identificador único
  - `nombre`: Nombre comercial de la empresa
  - `razonSocial`: Razón social oficial
  - `cuil`: CUIL (Código Único de Identificación Laboral)
  - `sucursales`: Conjunto de sucursales que pertenecen a la empresa

#### 6. **Sucursal** (`src/entidades/Sucursal.java`)
- **Propósito**: Representa una sucursal específica de una empresa
- **Atributos**:
  - `id`: Identificador único
  - `nombre`: Nombre de la sucursal
  - `horarioApertura`: Hora de apertura (LocalTime)
  - `horarioCierre`: Hora de cierre (LocalTime)
  - `esCasaMatriz`: Indica si es la casa matriz
  - `domicilio`: Referencia al domicilio donde se encuentra
  - `empresa`: Referencia a la empresa propietaria

### Patrón Repository

#### **InMemoryRepository** (`src/repositorios/InMemoryRepository.java`)
Implementa el patrón Repository para el manejo de datos en memoria:

- **Operaciones CRUD**:
  - `save(T entity)`: Guarda una entidad y genera ID automáticamente
  - `findById(Long id)`: Busca una entidad por su ID
  - `findAll()`: Obtiene todas las entidades
  - `genericUpdate(Long id, T updatedEntity)`: Actualiza una entidad existente
  - `genericDelete(Long id)`: Elimina una entidad por ID
  - `genericFindByField(String fieldName, Object value)`: Búsqueda genérica por campo

- **Características técnicas**:
  - Uso de reflexión para métodos `setId()` y `get[Field]()`
  - Generación automática de IDs con `AtomicLong`
  - Manejo de excepciones con try-catch
  - Almacenamiento en memoria con `HashMap<Long, T>`

## 🛠️ Tecnologías Utilizadas

- **Java**: Lenguaje de programación principal
- **Lombok**: Para reducir código boilerplate con anotaciones:
  - `@AllArgsConstructor`, `@NoArgsConstructor`: Constructores
  - `@Setter`, `@Getter`: Métodos de acceso
  - `@SuperBuilder`: Patrón Builder
  - `@ToString`: Método toString personalizado
- **Java Time API**: Para manejo de horarios (`LocalTime`)
- **Collections Framework**: `HashSet`, `HashMap`, `ArrayList`, `Optional`

## 📁 Estructura del Proyecto

```
src/
├── entidades/
│   ├── Domicilio.java      # Entidad de dirección física
│   ├── Empresa.java        # Entidad de empresa
│   ├── Localidad.java      # Entidad de localidad
│   ├── Pais.java          # Entidad de país
│   ├── Provincia.java     # Entidad de provincia
│   └── Sucursal.java      # Entidad de sucursal
├── repositorios/
│   └── InMemoryRepository.java  # Implementación del patrón Repository
└── Main.java             # Clase principal con casos de prueba
```

## 🚀 Funcionalidades Implementadas

### Casos de Prueba en Main.java

1. **Creación de estructura geográfica**:
   - País: Argentina
   - Provincias: Buenos Aires, Córdoba
   - Localidades: CABA, La Plata, Córdoba Capital, Villa Carlos Paz

2. **Creación de domicilios**:
   - Domicilios asociados a diferentes localidades
   - Información completa de dirección (calle, número, CP, piso, departamento)

3. **Creación de sucursales**:
   - Sucursales con horarios de atención
   - Identificación de casa matriz
   - Asociación con domicilios específicos

4. **Creación de empresas**:
   - Empresas con información legal completa
   - Asociación con múltiples sucursales
   - Relaciones bidireccionales empresa-sucursal

5. **Operaciones del repositorio**:
   - Guardado de entidades
   - Búsqueda por ID
   - Búsqueda por campo específico
   - Actualización de entidades
   - Eliminación de entidades
   - Consulta de sucursales por empresa

## 🔧 Características Técnicas Destacadas

### Manejo de Relaciones Bidireccionales
- **Empresa ↔ Sucursal**: Una empresa tiene múltiples sucursales, cada sucursal pertenece a una empresa
- **Pais ↔ Provincia ↔ Localidad**: Jerarquía geográfica completa
- **Domicilio → Localidad**: Cada domicilio pertenece a una localidad específica

### Prevención de Recursión Infinita
- Uso de `@ToString(exclude = "campo")` en Lombok
- Implementación manual de `toString()` para evitar referencias circulares
- Manejo cuidadoso de relaciones bidireccionales

### Patrón Builder
- Implementación con `@SuperBuilder` de Lombok
- Construcción fluida de objetos complejos
- Valores por defecto con `@Builder.Default`

### Repository Genérico
- Implementación genérica que funciona con cualquier entidad
- Uso de reflexión para operaciones dinámicas
- Manejo automático de IDs
- Operaciones CRUD completas

## 📊 Ejemplo de Uso

```java
// Crear repositorio
InMemoryRepository<Empresa> empresaRepository = new InMemoryRepository<>();

// Crear empresa con sucursales
Empresa empresa = Empresa.builder()
    .nombre("Mi Empresa")
    .razonSocial("Mi Empresa S.A.")
    .cuil(12345678901L)
    .sucursales(sucursalesSet)
    .build();

// Guardar en repositorio
empresaRepository.save(empresa);

// Buscar por ID
Optional<Empresa> encontrada = empresaRepository.findById(1L);

// Buscar por nombre
List<Empresa> porNombre = empresaRepository.genericFindByField("nombre", "Mi Empresa");
```

## 🎯 Objetivos del Ejercicio

1. **Modelado de Dominio**: Demostrar el diseño de un modelo de dominio complejo con múltiples entidades relacionadas
2. **Patrones de Diseño**: Implementar Builder Pattern y Repository Pattern
3. **Relaciones Bidireccionales**: Manejar correctamente las relaciones entre entidades
4. **Operaciones CRUD**: Implementar operaciones básicas de persistencia
5. **Reflexión Java**: Utilizar reflexión para operaciones genéricas
6. **Manejo de Colecciones**: Trabajar con diferentes tipos de colecciones Java

## 📝 Notas de Implementación

- Todas las entidades implementan `toString()` personalizado para evitar recursión infinita
- El repositorio utiliza reflexión para operaciones genéricas, asumiendo convenciones de nombres estándar
- Los IDs se generan automáticamente usando `AtomicLong` para garantizar unicidad
- Las relaciones bidireccionales se mantienen consistentes mediante asignación explícita

Este ejercicio demuestra conceptos fundamentales de programación orientada a objetos, patrones de diseño y el manejo de relaciones complejas entre entidades en Java.
