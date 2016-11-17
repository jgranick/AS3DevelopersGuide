## Enabling text selection, editing, and undo with TLF {#enabling-text-selection-editing-and-undo-with-tlf}

Flash Player 9.0 and later, Adobe AIR 1.0 and later

The ability to select or edit text is controlled at the text flow level. Every instance of the TextFlow class has an associated interaction manager. You can access a TextFlow object’s interaction manager through the object’s TextFlow.interactionManager property. To enable text selection, assign an instance of the SelectionManager class to the interactionManager property. To enable both text selection and editing, assign an instance of the EditManager class instead of an instance of the SelectionManager class. To enable undo operations, create an instance of the UndoManager class and include it as an argument when calling the constructor for EditManager. The UndoManager class maintains a history of the user&#039;s most recent editing activities and lets the user undo or redo specific edits. All three of these classes are part of the edit package.

For more information, see SelectionManager, EditManager, and UndoManager in the ActionScript 3.0 Reference for the Adobe Flash Platform.