# Image-Filter-And-Editing-Tools_
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image, ImageTk, ImageEnhance, ImageOps

class ImageEditor:
    def __init__(self, root):
        self.root = root
        self.root.title("Image Filter and Editing Tool")
        self.root.geometry("800x600")
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image, ImageTk, ImageEnhance, ImageOps

class ImageEditor:
    def __init__(self, root):
        self.root = root
        self.root.title("Image Filter and Editing Tool")
        self.root.geometry("800x600")
        
        # Initialize variables
        self.img = None
        self.img_display = None
        self.filepath = None

        # UI Layout
        self.create_widgets()

    def create_widgets(self):
        # Buttons for loading and saving images
        tk.Button(self.root, text="Load Image", command=self.load_image).pack(side="top", pady=10)
        tk.Button(self.root, text="Save Image", command=self.save_image).pack(side="top", pady=5)
        
        # Canvas for image display
        self.canvas = tk.Canvas(self.root, width=600, height=400, bg="gray")
        self.canvas.pack(pady=20)

        # Filters
        tk.Button(self.root, text="Grayscale", command=self.apply_grayscale).pack(side="left", padx=10)
        tk.Button(self.root, text="Sepia", command=self.apply_sepia).pack(side="left", padx=10)
        tk.Button(self.root, text="Invert", command=self.apply_invert).pack(side="left", padx=10)

        # Sliders for Brightness and Contrast
        self.brightness_scale = tk.Scale(self.root, from_=0.5, to=2, orient="horizontal", resolution=0.1, label="Brightness", command=self.adjust_brightness)
        self.brightness_scale.set(1)
        self.brightness_scale.pack(side="left", padx=10)

        self.contrast_scale = tk.Scale(self.root, from_=0.5, to=2, orient="horizontal", resolution=0.1, label="Contrast", command=self.adjust_contrast)
        self.contrast_scale.set(1)
        self.contrast_scale.pack(side="left", padx=10)

    def load_image(self):
        # Load an image file
        self.filepath = filedialog.askopenfilename(filetypes=[("Image files", "*.jpg;*.jpeg;*.png")])
        if self.filepath:
            self.img = Image.open(self.filepath)
            self.display_image(self.img)

    def save_image(self):
        # Save the modified image
        if self.img:
            save_path = filedialog.asksaveasfilename(defaultextension=".jpg", filetypes=[("JPEG file", "*.jpg"), ("PNG file", "*.png")])
            if save_path:
                self.img.save(save_path)
                messagebox.showinfo("Image Saved", "Your edited image has been saved successfully!")

    def display_image(self, img):
        # Display image on the canvas
        img_resized = img.resize((600, 400))
        self.img_display = ImageTk.PhotoImage(img_resized)
        self.canvas.create_image(300, 200, image=self.img_display)

    def apply_grayscale(self):
        # Apply grayscale filter
        if self.img:
            self.img = ImageOps.grayscale(self.img)
            self.display_image(self.img)

    def apply_sepia(self):
        # Apply sepia filter
        if self.img:
            sepia_img = ImageOps.colorize(ImageOps.grayscale(self.img), "#704214", "#C0C080")
            self.img = sepia_img
            self.display_image(self.img)

    def apply_invert(self):
        # Apply invert colors filter
        if self.img:
            self.img = ImageOps.invert(self.img.convert("RGB"))
            self.display_image(self.img)

    def adjust_brightness(self, val):
        # Adjust brightness
        if self.img:
            enhancer = ImageEnhance.Brightness(self.img)
            img_bright = enhancer.enhance(float(val))
            self.display_image(img_bright)

    def adjust_contrast(self, val):
        # Adjust contrast
        if self.img:
            enhancer = ImageEnhance.Contrast(self.img)
            img_contrast = enhancer.enhance(float(val))
            self.display_image(img_contrast)

# Main loop
root = tk.Tk()
app = ImageEditor(root)
root.mainloop()
