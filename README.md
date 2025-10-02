# data_structures_and_algorithms.py
#
# Comprehensive file containing Python code for all Data Structures and Algorithms
# practicals (1 through 8) as extracted from your course journal.
# This file is suitable for uploading to GitHub.
#
# NOTE: This file includes demonstration blocks using the if __name__ == "__main__":
# structure. Run this file directly to see all the code in action.
#

import sys
from array import array
from collections import deque

# ==============================================================================
# PRACTICAL 1: ARRAY OPERATIONS
# ==============================================================================

def array_insert(arr_list, element, position):
    """P1-A: Insert an element at a specific position (0-based) in an array/list."""
    try:
        # Using a regular Python list for flexibility
        arr_list.insert(position, element)
        print(f"  Inserted {element} at position {position}.")
        print(f"  Result: {arr_list}")
    except IndexError:
        print(f"  Error: Position {position} is invalid for insertion.")
    return arr_list

def array_delete(arr_list, position):
    """P1-B: Delete an element from a specific position (0-based) from an array/list."""
    try:
        if 0 <= position < len(arr_list):
            deleted_element = arr_list.pop(position)
            print(f"  Deleted {deleted_element} from position {position}.")
            print(f"  Result: {arr_list}")
        else:
            print(f"  Error: Position {position} is out of bounds for deletion.")
    except Exception as e:
        print(f"  An error occurred: {e}")
    return arr_list

def array_linear_search(arr_list, key):
    """P1-C: Search for an element in an array using Linear Search."""
    for i in range(len(arr_list)):
        if arr_list[i] == key:
            print(f"  Element {key} found at position {i}.")
            return i
    print(f"  Element {key} not found.")
    return -1

# ==============================================================================
# PRACTICAL 2: LINKED LIST OPERATIONS (Using dictionary-based node)
# ==============================================================================

def ll_display(head):
    """Helper function to traverse and print the linked list."""
    temp = head
    path = []
    while temp is not None:
        path.append(str(temp.get('data')))
        temp = temp.get('next')
    print("  List: " + " -> ".join(path) + " -> None")

def ll_insert_begin(head, value):
    """P2-B: Insert a node at the beginning."""
    new_node = {'data': value, 'next': head}
    return new_node

def ll_insert_end(head, value):
    """P2-B: Insert a node at the end."""
    new_node = {'data': value, 'next': None}
    if head is None:
        return new_node
    
    temp = head
    while temp['next'] is not None:
        temp = temp['next']
    temp['next'] = new_node
    return head

def ll_insert_pos(head, value, pos):
    """P2-B: Insert a new node at a given 0-based position."""
    if pos == 0:
        return ll_insert_begin(head, value)
    
    new_node = {'data': value, 'next': None}
    temp = head
    
    # Traverse to the node *before* the insertion point (pos - 1)
    for _ in range(pos - 1):
        if temp is None or temp['next'] is None:
            print(f"  Warning: Position {pos} out of bounds, inserting at end.")
            break
        temp = temp['next']
    
    new_node['next'] = temp['next']
    temp['next'] = new_node
    return head

def ll_delete_at_pos(head, pos):
    """P2-C: Delete a node from a given 0-based position."""
    if head is None:
        print("  List is empty, nothing to delete.")
        return None

    # Case 1: Deleting the head (position 0)
    if pos == 0:
        return head['next']

    temp = head
    
    # Traverse to the node *before* the deletion point (pos - 1)
    for _ in range(pos - 1):
        if temp is None or temp['next'] is None:
            print(f"  Error: Position {pos} out of bounds, no deletion performed.")
            return head
        temp = temp['next']

    # Case 2: Deletion in the middle or end
    if temp is not None and temp['next'] is not None:
        print(f"  Deleted node with data: {temp['next']['data']} at position {pos}")
        temp['next'] = temp['next']['next']
    
    return head

# ==============================================================================
# PRACTICAL 3: STACK OPERATIONS (LIFO)
# ==============================================================================

# Stack for P3-A
stack_list = []

def stack_push(item):
    """P3-A: Adds an item to the top of the stack."""
    stack_list.append(item)
    print(f"  Pushed: {item}. Current Stack: {stack_list}")

def stack_pop():
    """P3-A: Removes and returns the item from the top of the stack."""
    if not stack_list:
        return "Stack is empty"
    
    popped_item = stack_list.pop()
    print(f"  Popped: {popped_item}. Current Stack: {stack_list}")
    return popped_item

def precedence(op):
    """P3-B: Helper for operator precedence."""
    if op in ['+', '-']:
        return 1
    if op in ['*', '/']:
        return 2
    if op in ['^']:
        return 3
    return 0

def infix_to_postfix(exp):
    """P3-B: Convert an infix expression to a postfix expression using a stack."""
    op_stack = []
    result = ""
    
    for ch in exp:
        if ch.isalnum():
            result += ch
        elif ch == '(':
            op_stack.append(ch)
        elif ch == ')':
            while op_stack and op_stack[-1] != '(':
                result += op_stack.pop()
            if op_stack and op_stack[-1] == '(':
                op_stack.pop() # Pop the '('
        elif ch in ['+', '-', '*', '/', '^']:
            while (op_stack and 
                   precedence(op_stack[-1]) >= precedence(ch) and 
                   op_stack[-1] != '('):
                   
