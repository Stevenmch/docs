## Diagrama de Flujo

```mermaid
graph TD
    A[Inicio: DOMContentLoaded] --> B{Obtener elementos: dropArea, placeholder, sendButton};
    B --> C{Definir funcion updatePlaceholder()};
    C --> D{dropArea contiene texto?};
    D -- Si --> E[Ocultar placeholder, habilitar sendButton, anadir clase "enabled"];
    D -- No --> F[Mostrar placeholder, deshabilitar sendButton, quitar clase "enabled"];
    E --> G[Asignar listeners a dropArea: input, paste];
    F --> G;
    G --> H[Llamar updatePlaceholder() inicialmente];
    H --> I[Asignar listener click a sendButton];
    I --> J{Extraer codigo de dropArea (sin espacios)};
    J --> K[Imprimir codigo en consola (o enviar a API)];
    K --> L[Fin];
