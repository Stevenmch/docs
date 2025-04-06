## Diagrama de Flujo

```mermaid
graph LR
A[Carga del DOM] --> B{Contenido en dropArea?};
B -- Sí --> C[Ocultar placeholder];
B -- No --> D[Mostrar placeholder];
C --> E[Habilitar sendButton];
D --> F[Deshabilitar sendButton];
E --> G[Click en sendButton];
F --> G
G --> H[Capturar código];
H --> I[Enviar código];
