## Diagrama de Flujo

```mermaid
graph LR
A[Carga del DOM] --> B{Contenido en dropArea?};
B -- Sí --> C[Ocultar placeholder];
C --> D[Habilitar sendButton];
B -- No --> E[Mostrar placeholder];
E --> F[Deshabilitar sendButton];
D --> G[Escuchar eventos input/paste];
F --> G
G --> H{Evento input/paste};
H -- input --> I[Actualizar placeholder];
H -- paste --> J[Esperar];
J --> I
I --> B
G --> K[Click en sendButton];
K --> L[Extraer código];
L --> M[Imprimir código];
