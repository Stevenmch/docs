graph TD
    A[DOMContentLoaded Event] --> B{Get Elements: dropArea, placeholder, sendButton};
    B --> C{updatePlaceholder Function};
    C --> D{Check if dropArea has content};
    D -- Yes --> E[Hide placeholder, Enable sendButton];
    D -- No --> F[Show placeholder, Disable sendButton];
    B --> G{Attach 'input' event listener to dropArea};
    B --> H{Attach 'paste' event listener to dropArea};
    G --> C;
    H --> I[setTimeout(updatePlaceholder, 0)];
    I --> C;
    B --> J[Attach 'click' event listener to sendButton};
    J --> K{Get code from dropArea};
    K --> L[Log code to console];
    L --> M[Send code to backend/API (placeholder)];
