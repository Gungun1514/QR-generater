# QR-generater
import tkinter as tk
from tkinter import messagebox, filedialog
import qrcode
from PIL import Image, ImageTk


def generate_qr():
    """Generate QR code from the user input text."""
    text = text_entry.get()
    if not text.strip():
        messagebox.showerror("Error", "Input field cannot be empty!")
        return

     try:
        qr = qrcode.QRCode(version=1, error_correction=qrcode.constants.ERROR_CORRECT_H, box_size=10, border=4)
        qr.add_data(text)
        qr.make(fit=True)
        qr_image = qr.make_image(fill_color="black", back_color="white")

        global qr_code_image
        qr_code_image = qr_image

        qr_image_tk = ImageTk.PhotoImage(qr_image)
        qr_display_label.config(image=qr_image_tk)
        qr_display_label.image = qr_image_tk

        save_button.config(state=tk.NORMAL)
        messagebox.showinfo("Success", "QR code generated successfully!")
    except Exception as e:
        messagebox.showerror("Error", f"Failed to generate QR code: {e}")


def save_qr():
    """Save the generated QR code as a PNG file."""
    file_path = filedialog.asksaveasfilename(defaultextension=".png", filetypes=[("PNG Files", "*.png")])
    if file_path:
        try:
            qr_code_image.save(file_path)
            messagebox.showinfo("Success", f"QR code saved successfully at {file_path}")
        except Exception as e:
            messagebox.showerror("Error", f"Failed to save QR code: {e}")


# GUI setup
root = tk.Tk()
root.title("QR Code Generator")
root.geometry("400x500")
root.resizable(False, False)

# Input field and button
text_label = tk.Label(root, text="Enter text or URL:", font=("Arial", 12))
text_label.pack(pady=10)

text_entry = tk.Entry(root, width=40, font=("Arial", 12))
text_entry.pack(pady=5)

generate_button = tk.Button(root, text="Generate QR Code", font=("Arial", 12), command=generate_qr)
generate_button.pack(pady=10)

# QR code display area
qr_display_label = tk.Label(root)
qr_display_label.pack(pady=20)

# Save button
save_button = tk.Button(root, text="Save as PNG", font=("Arial", 12), command=save_qr)
save_button.pack(pady=10)

# Run the application
root.mainloop()
