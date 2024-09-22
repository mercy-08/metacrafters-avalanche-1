# Todo Smart Contract

This Solidity program is a simple Todo List contract that allows the contract owner to create, update, mark, and delete tasks. It demonstrates essential Solidity concepts such as modifiers, data structures, and error handling. The contract is designed to be managed by the contract's owner and provides functionality for managing multiple tasks efficiently.

## Description

The Todo Smart Contract is written in Solidity and provides the basic functionality for managing a todo list on the Ethereum blockchain. The owner of the contract can create new tasks, update them, mark them as completed, and delete them. Non-owners are restricted from performing these actions through the use of a modifier that ensures only the owner can modify the list. The contract also supports retrieving the details of a specific task or all tasks at once.

## Getting Started

### Executing Program

To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at [https://remix.ethereum.org](https://remix.ethereum.org).

Once you are on the Remix website, create a new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a `.sol` extension (e.g., `Todo.sol`). Copy and paste the following code into the file:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.22;

contract Todo {

    error ONLY_THE_OWNER_CAN_CALL_THIS_FUNCTION();
    
    struct TodoList{
        string title;
        string description;
        bool isCompleted;
    }

    TodoList[] public todo;

    address private owner;

    constructor() {
        owner = msg.sender;
    }

    function onlyOwner() private view {
        if (msg.sender != owner){
            revert ONLY_THE_OWNER_CAN_CALL_THIS_FUNCTION();
        }
    }

    function createTodo(string calldata _title, string calldata _description) external  {
        onlyOwner();
        todo.push(TodoList(_title,_description,false));
    }

    function updateTodo(uint _index, string calldata _title, string calldata _description) external  {
        onlyOwner();
        require(_index < todo.length, "Index out of bounds.");
        todo[_index] = TodoList(_title, _description, false);
    }

    function markTodo(uint _index) external  {
        onlyOwner();
        require(_index < todo.length, "Index out of bounds.");
        todo[_index].isCompleted =!todo[_index].isCompleted;
    }

    function getTodo(uint _index) external view returns(string memory, string memory, bool){
        require(_index < todo.length, "Index out of bounds.");
        TodoList memory todos = todo[_index];
        return (todos.title, todos.description, todos.isCompleted);
    }

    function deleteTodo(uint _index) external  {
        onlyOwner();
        require(_index < todo.length, "Index out of bounds.");
        delete todo[_index];
    }

    function getAllTodo() external view returns(TodoList[] memory){
        return todo;
    }

    // Internal consistency check using assert
    function checkConsistency() external view {
        onlyOwner();
        uint256 expectedLength = todo.length;
        for (uint i = 0; i < todo.length; i++) {
            assert(todo[i].isCompleted == false || todo[i].isCompleted == true);
        }
        if (expectedLength!= todo.length) {
            revert("Array length inconsistency detected.");
        }
    }
}
```

### Compilation and Deployment

1. Compile the contract:
   - Click on the "Solidity Compiler" tab in the left-hand sidebar.
   - Ensure the "Compiler" version is set to `0.8.22` (or a compatible version).
   - Click the "Compile Todo.sol" button.

2. Deploy the contract:
   - Go to the "Deploy & Run Transactions" tab in the left-hand sidebar.
   - Select the "Todo" contract from the dropdown menu.
   - Click the "Deploy" button.

3. Interact with the contract:
   - Use the deployed contract functions to create, update, mark, delete, and retrieve todo tasks.

## Authors

Mercy Omemgboji  
[@mercy-08](https://github.com/mercy-08)
