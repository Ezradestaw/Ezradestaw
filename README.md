import tkinter as tk
from tkinter import messagebox
import pyperclip
import random

# Zero-width characters to insert randomly
ZERO_WIDTH = ["\u200b", "\u200c", "\u200d", "\u2060"]

# Look-alike mapping using Cyrillic, Greek, Persian, etc.
SIMILAR_CHARS = {
    "A": ["А", "Ꭺ", "Λ"],
    "B": ["В", "ϐ"],
    "C": ["С", "Ϲ"],
    "D": ["Ꭰ"],
    "E": ["Е", "ℰ", "Ξ"],
    "F": ["Ғ"],
    "G": ["ɢ", "Ԍ"],
    "H": ["Н", "Η", "Һ"],
    "I": ["І", "Ӏ", "Ι"],
    "J": ["Ј"],
    "K": ["К", "Қ"],
    "L": ["Ꮮ", "Ŀ"],
    "M": ["М", "Ϻ"],
    "N": ["Ν", "П"],
    "O": ["О", "Θ", "Օ"],
    "P": ["Р", "Ρ", "Ⲣ"],
    "Q": ["Ԛ"],
    "R": ["Ꮢ", "Ʀ", "Я"],
    "S": ["Ѕ", "Տ", "Ꮪ"],
    "T": ["Т", "Ƭ", "Ⲧ"],
    "U": ["Ս", "Ս"],
    "V": ["Ѵ", "Ṽ"],
    "W": ["Ԝ", "Ѡ"],
    "X": ["Х", "Χ", "Ҳ"],
    "Y": ["Ү", "Υ", "Ұ"],
    "Z": ["Ζ", "Ȥ"],
    "a": ["а", "ɑ", "α"],
    "b": ["Ь", "Ƅ"],
    "c": ["с", "ϲ", "ƈ"],
    "d": ["ԁ", "ժ"],
    "e": ["е", "ℯ", "ҽ"],
    "f": ["ғ"],
    "g": ["ɡ", "ց"],
    "h": ["һ", "հ"],
    "i": ["і", "ɩ", "ι"],
    "j": ["ј", "ʝ"],
    "k": ["κ", "ᴋ", "қ"],
    "l": ["ⅼ", "ӏ"],
    "m": ["м", "ɱ"],
    "n": ["ո", "ո"],
    "o": ["о", "օ", "σ"],
    "p": ["р", "ρ"],
    "q": ["ԛ", "զ"],
    "r": ["г", "ᴦ", "ř"],
    "s": ["ѕ", "ʂ", "ս"],
    "t": ["т", "է"],
    "u": ["υ", "ս"],
    "v": ["ѵ", "ν"],
    "w": ["ԝ", "ɯ"],
    "x": ["х", "χ"],
    "y": ["у", "ү"],
    "z": ["ᴢ", "ʐ"]
}

def obfuscate_text(text):
    result = ""
    for ch in text:
        if ch in SIMILAR_CHARS:
            # Replace with a random similar character
            new_ch = random.choice(SIMILAR_CHARS[ch])
        else:
            new_ch = ch
        # Randomly add invisible characters
        if random.random() < 0.25:
            new_ch += random.choice(ZERO_WIDTH)
        result += new_ch
    return result

def process_text():
    original = input_text.get("1.0", tk.END).strip()
    if not original:
        messagebox.showwarning("Warning", "Please enter some text.")
        return
    changed = obfuscate_text(original)
    output_text.delete("1.0", tk.END)
    output_text.insert(tk.END, changed)

def copy_to_clipboard():
    text = output_text.get("1.0", tk.END).strip()
    if not text:
        messagebox.showwarning("Warning", "No text to copy.")
    else:
        pyperclip.copy(text)
        messagebox.showinfo("Copied", "Changed text copied to clipboard!")

# GUI Setup
root = tk.Tk()
root.title("🌀 Fancy Text Obfuscator")
root.geometry("800x600")
root.configure(bg="#111")

title = tk.Label(root, text="Fancy Text Obfuscator", fg="white", bg="#111", font=("Arial", 18, "bold"))
title.pack(pady=10)

frame = tk.Frame(root, bg="#111")
frame.pack(padx=10, pady=10, fill="both", expand=True)

tk.Label(frame, text="Enter your passage:", fg="white", bg="#111").pack(anchor="w")
input_text = tk.Text(frame, height=10, wrap="word", font=("Consolas", 12))
input_text.pack(fill="x", pady=5)

tk.Button(frame, text="Convert", command=process_text, bg="#444", fg="white", font=("Arial", 12, "bold")).pack(pady=10)

tk.Label(frame, text="Changed Text:", fg="white", bg="#111").pack(anchor="w")
output_text = tk.Text(frame, height=10, wrap="word", font=("Consolas", 12))
output_text.pack(fill="x", pady=5)

tk.Button(frame, text="Copy to Clipboard", command=copy_to_clipboard, bg="#0078D7", fg="white", font=("Arial", 12, "bold")).pack(pady=10)

root.mainloop()
