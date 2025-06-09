# Micro-IT-StopClock
import tkinter as tk
from datetime import datetime
import time

class StopwatchClockApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Stopwatch & Clock")
        self.root.geometry("400x300")
        
        # Stopwatch variables
        self.stopwatch_running = False
        self.stopwatch_time = 0
        self.start_time = 0
        
        # Create and configure main frame
        self.frame = tk.Frame(self.root)
        self.frame.pack(pady=20)
        
        # Clock display
        self.clock_label = tk.Label(
            self.frame, 
            text="00:00:00", 
            font=("Arial", 24),
            bg="black",
            fg="white",
            width=10
        )
        self.clock_label.pack(pady=10)
        
        # Stopwatch display
        self.stopwatch_label = tk.Label(
            self.frame, 
            text="00:00:00.000", 
            font=("Arial", 24),
            bg="black",
            fg="white",
            width=12
        )
        self.stopwatch_label.pack(pady=10)
        
        # Buttons frame
        self.button_frame = tk.Frame(self.frame)
        self.button_frame.pack(pady=10)
        
        # Buttons
        self.start_button = tk.Button(
            self.button_frame, 
            text="Start", 
            command=self.start_stopwatch,
            width=10
        )
        self.start_button.grid(row=0, column=0, padx=5)
        
        self.stop_button = tk.Button(
            self.button_frame, 
            text="Stop", 
            command=self.stop_stopwatch,
            width=10
        )
        self.stop_button.grid(row=0, column=1, padx=5)
        
        self.reset_button = tk.Button(
            self.button_frame, 
            text="Reset", 
            command=self.reset_stopwatch,
            width=10
        )
        self.reset_button.grid(row=0, column=2, padx=5)
        
        # Start clock update
        self.update_clock()
        
    def update_clock(self):
        current_time = datetime.now().strftime("%H:%M:%S")
        self.clock_label.config(text=current_time)
        self.root.after(1000, self.update_clock)
        
    def update_stopwatch(self):
        if self.stopwatch_running:
            elapsed = time.time() - self.start_time + self.stopwatch_time
            hours = int(elapsed // 3600)
            minutes = int((elapsed % 3600) // 60)
            seconds = int(elapsed % 60)
            milliseconds = int((elapsed % 1) * 1000)
            self.stopwatch_label.config(
                text=f"{hours:02d}:{minutes:02d}:{seconds:02d}.{milliseconds:03d}"
            )
            self.root.after(10, self.update_stopwatch)
        
    def start_stopwatch(self):
        if not self.stopwatch_running:
            self.start_time = time.time()
            self.stopwatch_running = True
            self.update_stopwatch()
            self.start_button.config(state="disabled")
            self.stop_button.config(state="normal")
        
    def stop_stopwatch(self):
        if self.stopwatch_running:
            self.stopwatch_time += time.time() - self.start_time
            self.stopwatch_running = False
            self.start_button.config(state="normal")
            self.stop_button.config(state="disabled")
        
    def reset_stopwatch(self):
        self.stopwatch_running = False
        self.stopwatch_time = 0
        self.stopwatch_label.config(text="00:00:00.000")
        self.start_button.config(state="normal")
        self.stop_button.config(state="normal")

if __name__ == "__main__":
    root = tk.Tk()
    app = StopwatchClockApp(root)
    root.mainloop()
