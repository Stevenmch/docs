## Diagrama de Flujo

```mermaid
graph TD
    A[Inicio] --> B[Obtener elementos];
    B --> C[Definir función updatePlaceholder];
    C --> D{¿dropArea contiene texto?};
    D -- Sí --> E[Ocultar placeholder, habilitar botón, añadir clase];
    D -- No --> F[Mostrar placeholder, deshabilitar botón, quitar clase];
    E --> G[Asignar listeners a dropArea];
    F --> G;
    G --> H[Llamar updatePlaceholder inicialmente];
    H --> I[Asignar listener click a botón];
    I --> J[Extraer código de dropArea];
    J --> K[Imprimir código en consola];
    K --> L[Fin];
