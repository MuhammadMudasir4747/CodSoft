# CodSofrom tkinter import *

# Function to add items in the list
def add_item(entry: Entry, listbox: Listbox):
    new_task = entry.get()
    if new_task.strip():
        listbox.insert(END, new_task)
        update_task_file(listbox)
        entry.delete(0, END)  # Clear the entry field

# Function to delete items from the list
def delete_item(listbox: Listbox):
    selected_index = listbox.curselection()
    if selected_index:
        listbox.delete(selected_index)
        update_task_file(listbox)

# Function to Edit items from the list
def edit_item(entry: Entry, listbox: Listbox):
    selected_index = listbox.curselection()
    if selected_index:
        selected_index = selected_index[0]
        current_text = listbox.get(selected_index)
        modified_text = entry.get()
        if modified_text.strip():
            listbox.delete(selected_index)
            listbox.insert(selected_index, modified_text)
            update_task_file(listbox)

def update_task_file(listbox: Listbox):
    tasks = listbox.get(0, END)
    with open('tasks.txt', 'w') as tasks_list_file:
        tasks_list_file.write('\n'.join(tasks))

root = Tk()
root.title('To-Do List')
root.geometry('300x425')

Label(root, text='To-Do List', bg='green', wraplength=300).place(x=120, y=0)

tasks = Listbox(root, selectbackground='Gold', bg='Silver', font=('Helvetica', 12), height=12, width=25)
scroller = Scrollbar(root, orient=VERTICAL, command=tasks.yview)
scroller.place(x=260, y=50, height=232)
tasks.config(yscrollcommand=scroller.set)
tasks.place(x=35, y=50)

with open('tasks.txt', 'r+') as tasks_list:
    for task in tasks_list:
        tasks.insert(END, task.strip())

new_item_entry = Entry(root, width=37)
new_item_entry.place(x=35, y=310)

add_btn = Button(root, text='Add Item', bg='Azure', width=10, font=('Helvetica', 12),
                 command=lambda: add_item(new_item_entry, tasks))
add_btn.place(x=45, y=350)

delete_btn = Button(root, text='Delete Item', bg='Azure', width=10, font=('Helvetica', 12),
                 command=lambda: delete_item(tasks))
delete_btn.place(x=150, y=350)

edit_button = Button(root, text='Edit Item', bg='Azure', width=10, font=('Helvetica', 12),
                    command=lambda: edit_item(new_item_entry, tasks))
edit_button.place(x=100, y=390)

root.update()
root.mainloop()ft
