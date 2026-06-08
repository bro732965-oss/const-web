# const-web
import tkinter as tk
from tkinter import colorchooser as cc
import webbrowser

root = tk.Tk()
root.title("GUI Constructor")
root.geometry("600x600")

erc = "black"
cmc = "gray"
last_text = ""
last_url = ""

def show_text():
    global erc, cmc, last_text, last_url
    cmd = entry.get().strip()
    
    # ===== ТВОИ ТЕГИ =====
    if cmd == "{color}!Tk!":
        color = cc.askcolor()[1]
        if color:
            root.configure(bg=color)
            label.config(text=f"Фон: {color}")
    
    elif cmd == "{color}!txt!":
        color = cc.askcolor()[1]
        if color:
            erc = color
            label.config(text=f"Цвет текста: {color}")
    
    elif cmd == "<new>#entry#":
        text_input = tk.Text(root, font=("Arial", 14), height=8, fg=erc)
        text_input.pack(fill="both", expand=True, padx=10, pady=5)
        label.config(text="Текстовое поле создано")
    
    elif cmd == "{color}#tag#":
        color = cc.askcolor()[1]
        if color:
            cmc = color
            label.config(text=f"Цвет кнопки: {color}")
    
    elif cmd.startswith("/txt/#tag#="):
        last_text = cmd.replace("/txt/#tag#=", "")
        label.config(text=f"Текст: {last_text}")
    
    # ===== НОВЫЙ ТЕГ ДЛЯ ССЫЛКИ =====
    elif cmd.startswith("/web#tag#="):
        last_url = cmd.replace("/web#tag#=", "")
        label.config(text=f"Ссылка: {last_url}")
    
    # Кнопка с открытием браузера
    elif cmd == "<new>#web#":
        if last_url:
            def open_browser():
                webbrowser.open(last_url)
            btn = tk.Button(root, text=last_text, command=open_browser, bg=cmc, fg="white")
            btn.pack(pady=5)
            label.config(text=f"Кнопка '{last_text}' открывает {last_url}")
        else:
            label.config(text="Ошибка: сначала /web#tag#=ССЫЛКА")
    
    # Старая версия (просто печатает в консоль)
    elif cmd == "<new>#tag#=web=":
        if last_text:
            btn = tk.Button(root, text=last_text, command=lambda: print(f"Нажата: {last_text}"), bg=cmc, fg="white")
            btn.pack(pady=5)
            label.config(text=f"Кнопка '{last_text}' создана")
        else:
            label.config(text="Ошибка: сначала /txt/#tag#=ТЕКСТ")
    
    elif cmd == "<new>#label#":
        lbl = tk.Label(root, text="Новая надпись", font=("Arial", 12))
        lbl.pack(pady=5)
        label.config(text="Надпись создана")
    
    elif cmd.startswith("/label#="):
        text = cmd.replace("/label#=", "")
        lbl = tk.Label(root, text=text, font=("Arial", 12), fg=erc)
        lbl.pack(pady=5)
        label.config(text=f"Надпись: {text}")
    
    elif cmd == "<new>#button#":
        if last_text:
            btn = tk.Button(root, text=last_text, command=lambda: print(f"Нажата: {last_text}"), bg=cmc, fg="white")
            btn.pack(pady=5)
        else:
            label.config(text="Сначала /txt/#tag#=ТЕКСТ")
    
    elif cmd == "<clear>":
        for widget in root.winfo_children():
            if widget not in [entry, button, label]:
                widget.destroy()
        label.config(text="Очищено")
    
    elif cmd == "#exp#":
        with open("swe.txt", "a") as f:
            f.write(cmd + "\n")
        label.config(text="Команда сохранена")
    
    elif cmd == "*open*":
        try:
            with open("swe.txt", "r") as f:
                content = f.read()
                label.config(text=f"Содержимое файла:\n{content[:200]}")
        except FileNotFoundError:
            label.config(text="Файл swe.txt не найден")
    
    else:
        label.config(text=f"??? {cmd}")
    
    entry.delete(0, tk.END)

# Интерфейс
entry = tk.Entry(root, width=50)
entry.pack(pady=10)

button = tk.Button(root, text="Выполнить", command=show_text, bg="red", fg="white")
button.pack(pady=5)

label = tk.Label(root, text="Команды:\n/txt/#tag#=ТЕКСТ\n/web#tag#=ССЫЛКА\n<new>#web#", wraplength=500, justify="left")
label.pack(pady=10)

root.mainloop()