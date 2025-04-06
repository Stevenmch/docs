## Diagrama de Flujo

```mermaid
graph TD
    A[Inicio: DOMContentLoaded] --> B{Area de texto vacia?};
    B -- Si --> C[Mostrar Placeholder];
    B -- No --> D[Ocultar Placeholder];
    C --> E{Boton habilitado?};
    D --> E;
    E -- Si --> F[Deshabilitar Boton];
    E -- No --> G[Habilitar Boton];
    F --> H[Escuchar eventos input/paste];
    G --> H;
    H --> I{Evento input/paste?};
    I -- Si --> B;
    I -- No --> J{Click en boton enviar?};
    J -- Si --> K[Capturar y procesar codigo];
    K --> L[Fin];
    J -- No --> H;
    L[Fin];
