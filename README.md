# To-Do-List-CLI-App-in-Python
This is a simple command-line interface (CLI) application to manage daily tasks efficiently. Users can add, view, complete, and delete tasks. All tasks are stored in a JSON file for persistence. The app runs in a loop until the user chooses to exit
# To-Do List CLI App in Python

import json

FILENAME = 'tasks.json'

def load_tasks():
    try:
        with open(FILENAME, 'r') as file:
            return json.load(file)
    except FileNotFoundError:
        return []

def save_tasks(tasks):
    with open(FILENAME, 'w') as file:
        json.dump(tasks, file, indent=4)

def display_tasks(tasks):
    if not tasks:
        print('No tasks found.')
        return
    for i, task in enumerate(tasks, 1):
        status = '✓' if task['completed'] else '✗'
        print(f"{i}. [{status}] {task['title']} (Priority: {task['priority']})")

def add_task(tasks):
    title = input('Enter task title: ')
    priority = input('Enter task priority (High, Medium, Low): ')
    tasks.append({'title': title, 'priority': priority, 'completed': False})
    save_tasks(tasks)
    print('Task added!')

def complete_task(tasks):
    display_tasks(tasks)
    index = int(input('Enter task number to mark complete: ')) - 1
    if 0 <= index < len(tasks):
        tasks[index]['completed'] = True
        save_tasks(tasks)
        print('Task marked as complete!')
    else:
        print('Invalid task number.')

def delete_task(tasks):
    display_tasks(tasks)
    index = int(input('Enter task number to delete: ')) - 1
    if 0 <= index < len(tasks):
        tasks.pop(index)
        save_tasks(tasks)
        print('Task deleted!')
    else:
        print('Invalid task number.')

def main():
    tasks = load_tasks()
    while True:
        print('\nTo-Do List Menu')
        print('1. View Tasks')
        print('2. Add Task')
        print('3. Mark Task Complete')
        print('4. Delete Task')
        print('5. Exit')
        choice = input('Choose an option: ')
        if choice == '1':
            display_tasks(tasks)
        elif choice == '2':
            add_task(tasks)
        elif choice == '3':
            complete_task(tasks)
        elif choice == '4':
            delete_task(tasks)
        elif choice == '5':
            break
        else:
            print('Invalid choice, please try again.')

if __name__ == '__main__':
    main()
