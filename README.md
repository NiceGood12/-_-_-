import tkinter as tk
from tkinter import scrolledtext
from collections import Counter
import pymorphy2

def normalize_word(word):
    morph = pymorphy2.MorphAnalyzer()
    parsed_word = morph.parse(word)[0]
    return parsed_word.normal_form

def count_words(*args, **kwargs):
    text = input_text.get("1.0", tk.END).split()
    normalized_text = [normalize_word(word.strip('.,!?').lower()) for word in text]
    filtered_text = [word for word in normalized_text if len(word) > 1]  # Отсекаем слова из одной буквы
    word_count = Counter(filtered_text)
    
    most_common_word = word_count.most_common(1)[0]
    least_common_word = min(word_count, key=word_count.get)
    
    result_text.delete("1.0", tk.END)
    result_text.insert(tk.END, f"Самое часто встречающееся слово: {most_common_word[0]} - {most_common_word[1]}\n")
    result_text.insert(tk.END, f"Самое редкое слово: {least_common_word}")

# Создаем графический интерфейс
root = tk.Tk()
root.title("Подсчет повторяющихся слов")

input_label = tk.Label(root, text="Введите текст:")
input_label.pack()

input_text = scrolledtext.ScrolledText(root, width=40, height=10)
input_text.pack()

count_button = tk.Button(root, text="Подсчитать", command=lambda: count_words())
count_button.pack()

result_label = tk.Label(root, text="Результат:")
result_label.pack()

result_text = scrolledtext.ScrolledText(root, width=40, height=4)
result_text.pack()

root.mainloop()
