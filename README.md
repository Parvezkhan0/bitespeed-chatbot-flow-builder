# Chatbot Flow Builder

A visual chatbot flow builder application built with React and React Flow that enables users to create, edit, and manage conversational flows through an intuitive drag-and-drop interface.

## 🚀 Live Demo

**[View Live Application](https://bitespeed-chatbot-flow-builder.onrender.com)**

## 📦 Repository

**[GitHub Repository](https://github.com/Parvezkhan0/bitespeed-chatbot-flow-builder)**

## Overview

This project implements a node-based editor for designing chatbot conversation flows. Users can add message nodes, connect them to define conversation paths, and configure message content through a dedicated settings panel. The application enforces validation rules to ensure all conversation flows are properly connected before saving.

## Core Features

### 1. Text Message Nodes
- **Multiple Node Support**: Create unlimited text message nodes within a single flow
- **Drag-and-Drop Creation**: Add new nodes by dragging from the nodes panel onto the canvas
- **Visual Representation**: Each node displays its message content and connection points
- **Interactive Editing**: Click any node to edit its content in the settings panel

### 2. Connection System
- **Source Handles**: Each node has a source connection point (right side)
  - Limited to **one outgoing connection** per node
  - Prevents creating multiple paths from a single message
- **Target Handles**: Each node has a target connection point (left side)
  - Supports **multiple incoming connections**
  - Allows branching conversation flows to converge

### 3. Nodes Panel
- Located on the right side of the interface
- Contains all available node types (currently Text Message)
- **Extensible Architecture**: Designed to accommodate additional node types in future versions
- Drag-and-drop interaction model for adding nodes to the canvas

### 4. Settings Panel
- Replaces the nodes panel when a node is selected
- Provides a text input field for editing message content
- Changes are reflected in real-time on the canvas
- Click outside or deselect to return to the nodes panel

### 5. Flow Validation & Saving
- **Save Button**: Persists the current flow configuration
- **Validation Rules**: 
  - If the flow contains more than one node, all nodes must be connected
  - Any node with an empty target handle (except the start node) triggers an error
  - Error notifications appear when validation fails
- **Success Feedback**: Confirmation message displayed on successful save

## Technical Architecture

### Technology Stack
- **React**: UI framework for component-based architecture
- **React Flow**: Specialized library for building node-based editors
- **Tailwind CSS**: Utility-first CSS framework for styling
- **Vite**: Build tool and development server

### Project Structure
```
src/
├── components/
│   ├── TextNode.jsx          # Custom text message node component
│   ├── NodesPanel.jsx         # Right sidebar with available nodes
│   ├── SettingsPanel.jsx      # Node editing interface
│   └── FlowCanvas.jsx         # Main React Flow canvas
├── utils/
│   └── validation.js          # Flow validation logic
├── hooks/
│   └── useFlowState.js        # Custom hook for flow state management
├── App.jsx                    # Root application component
└── main.jsx                   # Application entry point
```

### Key Design Decisions

#### State Management
The application uses React's built-in state management with custom hooks to handle:
- Node positions and data
- Edge connections between nodes
- Selected node tracking
- Panel visibility toggling

#### Validation Logic
The flow validation system checks two critical conditions:
1. **Connection Completeness**: When multiple nodes exist, every node except potential starting points must have at least one incoming connection
2. **Flow Integrity**: Prevents orphaned nodes that would never be reached during conversation execution

#### Extensibility Considerations
The architecture supports future expansion through:
- **Abstract Node Interface**: Node types implement a common interface, allowing new node types to be added without modifying core logic
- **Plugin-Ready Panels**: The panel system can accommodate new node categories and configuration options
- **Modular Validation**: Validation rules are separated into dedicated modules for easy extension

## Installation & Setup

### Prerequisites
- Node.js (version 14 or higher)
- npm or yarn package manager

### Local Development
```bash
# Clone the repository
git clone [repository-url]
cd chatbot-flow-builder

# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build
```

The development server runs on `http://localhost:5173` by default.

## Usage Guide

### Creating a Flow
1. **Add Nodes**: Drag the "Message" node from the right panel onto the canvas
2. **Position Nodes**: Arrange nodes to represent your conversation flow
3. **Create Connections**: Click and drag from a source handle (right side) to a target handle (left side)
4. **Edit Content**: Click any node to open the settings panel and modify its message

### Editing Nodes
1. Select a node by clicking on it
2. The settings panel appears with a text input
3. Type your message content
4. Changes apply immediately to the node
5. Click elsewhere or press the back button to deselect

### Saving Flows
1. Click the "Save" button in the top-right corner
2. If validation passes, you'll see a success message
3. If validation fails, an error describes the issue
4. Fix any validation errors and retry saving

### Validation Error Resolution
**Error: "Cannot save flow - Some nodes are not connected"**
- **Cause**: One or more nodes lack incoming connections
- **Solution**: Connect all isolated nodes to the flow, or remove unused nodes

## Implementation Details

### Connection Constraints
The single-source-handle constraint is enforced at the edge creation level. When a user attempts to create a second connection from a source handle:
```javascript
// Pseudo-code example
onConnect = (connection) => {
  const existingEdge = edges.find(e => 
    e.source === connection.source && 
    e.sourceHandle === connection.sourceHandle
  );
  
  if (existingEdge) {
    // Remove old edge before adding new one
    setEdges(edges.filter(e => e.id !== existingEdge.id));
  }
  
  setEdges([...edges, connection]);
}
```

### Validation Algorithm
The validation function performs these checks:
1. Count total nodes in the flow
2. If count ≤ 1, validation passes (single node is always valid)
3. If count > 1, check each node for incoming edges
4. Identify nodes with zero incoming connections
5. If any exist, validation fails with descriptive error

### Node Data Structure
Each node maintains this data structure:
```javascript
{
  id: "unique-identifier",
  type: "textNode",
  position: { x: 100, y: 100 },
  data: {
    label: "Message content goes here",
    onChange: (newLabel) => { /* update handler */ }
  }
}
```

## Future Enhancements

The current architecture supports these potential additions:
- **Additional Node Types**: Input capture, conditional branches, API calls, delays
- **Flow Templates**: Pre-built conversation templates for common use cases
- **Export/Import**: Save flows as JSON for backup or sharing
- **Flow Testing**: Simulate conversations to test flow logic
- **Analytics Integration**: Track which paths users take through conversations
- **Collaborative Editing**: Multiple users working on the same flow simultaneously
- **Version Control**: Track changes and revert to previous flow versions

## Known Limitations

- **No Persistence**: Flows are not saved to a backend (currently frontend-only)
- **Single User**: No authentication or multi-user support
- **Limited Node Types**: Only text messages are currently supported
- **No Undo/Redo**: Actions cannot be undone except by manual reversal
- **No Flow Testing**: Cannot simulate conversation flow execution

## Browser Compatibility

Tested and supported on:
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Performance Considerations

- Optimized for flows with up to 100 nodes
- Large flows (100+ nodes) may experience reduced responsiveness
- Connection validation runs on every save operation
- React Flow uses canvas rendering for optimal performance


---

**Note**: This is a frontend assessment project demonstrating React Flow integration, state management, and validation logic implementation. It serves as a foundation for a production chatbot builder with additional features and backend integration.
