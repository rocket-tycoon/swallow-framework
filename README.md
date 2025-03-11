# Swallow Framework

A lightweight, reactive Model-View-Command-Context (MVCC) framework for building 
structured applications with event-driven architecture.

## Overview

Swallow Framework provides a modern approach to application architecture by combining
the best aspects of MVC with reactive state management and the command pattern. It's 
designed to create maintainable, testable applications with clear separation of concerns.

### Key Features

- **Model-View-Command-Context Pattern**: A structured approach to application architecture
- **Reactive State Management**: Observable properties with automatic change detection and notification
- **Event-Driven Communication**: Components communicate through events rather than direct references
- **Command Pattern Implementation**: Actions encapsulated as commands for better testability
- **Lightweight and Flexible**: Minimal dependencies and adaptable to different application types

## Architecture

Swallow Framework is built on four main components:

### Model

Models manage application state using reactive properties. Any changes to these properties 
automatically notify registered listeners.

```python
from swallow_framework.mvcc.model import Model
from swallow_framework.state.property import state

class UserModel(Model):
    name = state("")
    age = state(0)
    
    def update_user(self, name, age):
        self.name = name
        self.age = age

# Listen for changes
user = UserModel()
user.on_change('name', lambda value: print(f"Name changed to: {value}"))
```

### View

Views handle user interaction and dispatch events through the context.

```python
from swallow_framework.mvcc.view import View
from swallow_framework.core.events import Event

class UserFormView(View):
    def submit_form(self, name, age):
        self.dispatch(Event("update_user", {"name": name, "age": age}))
```

### Command

Commands encapsulate actions to be performed on models.

```python
from swallow_framework.mvcc.command import Command

class UpdateUserCommand(Command):
    def execute(self, data):
        self.model.update_user(data["name"], data["age"])
```

### Context

The context maps events to commands and facilitates event dispatching.

```python
from swallow_framework.mvcc.context import Context
from swallow_framework.core.events import EventDispatcher

class AppContext(Context):
    def __init__(self):
        super().__init__(EventDispatcher())
        
        # Map events to commands
        user_model = UserModel()
        self.map_command("update_user", UpdateUserCommand(user_model))
```

## Installation

```bash
pip install git+https://github.com/rocket-tycoon/swallow-framework.git
```

You can also clone the repository and install it locally:

```bash
git clone https://github.com/rocket-tycoon/swallow-framework.git
cd swallow-framework
pip install -e .
```

## Getting Started

1. Create your models with reactive state properties
2. Define commands to encapsulate actions on models
3. Create a context that maps events to commands
4. Build views that dispatch events through the context

```python
# Create the application context
context = AppContext()

# Create a view with the context
view = UserFormView(context)

# Handle user interactions
view.submit_form("Alice", 30)
```

## Examples

Check the `examples/` directory for complete application examples using the Swallow Framework.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.