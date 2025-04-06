## Diagrama de Flujo

```mermaid
graph TD
    A[Inicio: DOMContentLoaded] --> B{Obtener elementos: dropArea, placeholder, sendButton};
    B --> C{Definir función updatePlaceholder()};
    C --> D{dropArea contiene texto?};
    D -- Sí --> E[Ocultar placeholder, habilitar sendButton, añadir clase "enabled"];
    D -- No --> F[Mostrar placeholder, deshabilitar sendButton, quitar clase "enabled"];
    E --> G[Asignar listeners de eventos a dropArea: input, paste];
    F --> G;
    G --> H[Llamar updatePlaceholder() inicialmente];
    H --> I[Asignar listener de evento click a sendButton];
    I --> J{Extraer código de dropArea (eliminar espacios)};
    J --> K[Imprimir código en la consola (o enviar a backend/API)];
    K --> L[Fin];
