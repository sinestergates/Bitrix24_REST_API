

from fast_bitrix24 import Bitrix
import tkinter as tk
from tkcalendar import Calendar
from tkinter import ttk

info_list = []
webhook_str = 'Your webhook'


def get_info(json: object) -> list:
    """Парсим нужные данные"""
    list_answer = []

    for i in json:
        name = i['NAME']
        time_in = i['DATE_FROM']
        time_out = i['DATE_TO']
        list = [name, time_in, time_out]
        list_answer.append(list)

    return list_answer



def connect() -> list:
    """Подключение к api Битрикс"""

    webhook = f"{entry_webhook.get()}"
    b = Bitrix(webhook)
    deals = b.get_all(
        'calendar.event.get',
        params={
            'type': 'user',
                'ownerId': f'{entry_id.get()}',
                'from': f'{cal_date_from.selection_get()}',
                'to': f'{cal_date_to.selection_get()}',
    })

    return deals

def tree_right_format ():
    """Форматируем таблицу"""
    tree_right.column('Текст записи', width=200, stretch='NO')
    tree_right.column('Выполнялось с', width=110, stretch='NO')
    tree_right.column('Выполнялось до', width=110, stretch='NO')
    tree_right.column('Оценка', width=50, stretch='NO')
    for i in range(4):
        tree_right.heading(i, text=tree_heading[i])


def tree_right_delete():

    """Очищаем таблицу"""
    for i in tree_right.get_children():
        tree_right.delete(i)

def insert_data_text():
    """Вносим данные в таблицу и текстовый виджет"""
    info_list.clear()
    tree_right_delete()
    deals = connect()
    var = get_info(deals)
    for i in var:
        info_list.append(i)
        text_widget.insert(1.0, i)
        text_widget.insert(1.0, '\n')
        tree_right.insert("", tk.END, values=i)

def update_tree_right(func_shoose: object =None):
    """Обновляем данные в таблице"""
    if func_shoose:
        func_shoose()



def choose_element_by_datatime(select_data: str):
    """Делаем подключение,парсим данные,
    находим нужную строку и добавляем данные о оценке"""

    for i in info_list:
        if i[1] == select_data:
            i.insert(3, estimation_entry.get())
    tree_right_delete()
    for i in info_list:
        tree_right.insert("", tk.END, values=i)

def select_tree_right(event: object=None):
    """SELECT таблицы tree_right"""
    if tree_right.selection():

        selected = tree_right.item(tree_right.selection()[0])['values']
        select_data = tree_right.item(tree_right.selection()[0])['values'][1]
        if estimation_entry.get():

            selected.append(estimation_entry.get())

        return {'selected': selected, 'select_data': select_data}



def estimation_run():
    """Запуск установки оценки"""
    tuple = select_tree_right()
    update_tree_right(func_shoose=choose_element_by_datatime(select_data=tuple['select_data']))


root = tk.Tk()
root.geometry('880x700')
entry_webhook = tk.Entry(root, width=28, bg='white', bd=4, font="Arial 10")
entry_id = tk.Entry(root, width=7, bg='white', bd=4, font="Arial 10")



tree_heading = ['Текст записи', 'Выполнялось с', 'Выполнялось до', 'Оценка']

entry_webhook.place(x=100, y=50)
entry_webhook.insert(tk.END, webhook_str)
entry_id.place(x=100, y=100)


title_WEBHOOK = tk.Label(root, text='WEBHOOK', font="Arial 10", background="#d9d9d9")
title_id = tk.Label(root, text='id работника', font="Arial 10", background="#d9d9d9")
title_date_before = tk.Label(root, text='Дата с', font="Arial 10", background="#d9d9d9")
title_date_after = tk.Label(root, text='Дата до', font="Arial 10", background="#d9d9d9")
title_WEBHOOK.place(x=20, y=50)
title_id.place(x=20, y=100)
title_date_before.place(x=90, y=128)
title_date_after.place(x=480, y=128)
text_widget = tk.Text(root, height=14, width=40)
text_widget.place(x=20, y=350)
cal_date_from = Calendar(root, selectmode='day',
               year=2022, month=12,
               day=22)
cal_date_to = Calendar(root, selectmode='day',
               year=2022, month=12,
               day=22)
cal_date_from.place(x=20, y=150)
cal_date_to.place(x=388, y=150)
tree_right = ttk.Treeview(root, show="headings", columns=tree_heading)
tree_right.bind("<<TreeviewSelect>>", select_tree_right)
tree_right.place(x=390, y=350)
Button_run = tk.Button(root, text='Запуск', font="Arial 11", width=12, height=2, bg='gray', fg='white',
                                     command=insert_data_text).place(x=320, y=50)
Button_run = tk.Button(root, text='Оценить', font="Arial 11", width=12, height=2, bg='gray', fg='white',
                                     command=estimation_run).place(x=470, y=580)
tree_right_format()
estimation_entry = tk.Entry(root, width=7, bg='white', bd=4, font="Arial 10")
estimation_entry.place(x=400, y=600)
root.mainloop()
