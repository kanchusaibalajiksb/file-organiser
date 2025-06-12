import os
import shutil

# Define file type folders
FILE_TYPES = {
    "Images": [".png", ".jpg", ".jpeg", ".gif"],
    "Documents": [".pdf", ".docx", ".txt", ".xlsx", ".pptx"],
    "Videos": [".mp4", ".mov", ".avi"],
    "Audio": [".mp3", ".wav"],
    "Others": []
}

def create_folders(base_path):
    for folder in FILE_TYPES:
        folder_path = os.path.join(base_path, folder)
        os.makedirs(folder_path, exist_ok=True)

def move_files(base_path):
    for file in os.listdir(base_path):
        file_path = os.path.join(base_path, file)
        if os.path.isfile(file_path):
            moved = False
            ext = os.path.splitext(file)[1].lower()
            for folder, extensions in FILE_TYPES.items():
                if ext in extensions:
                    shutil.move(file_path, os.path.join(base_path, folder, file))
                    moved = True
                    break
            if not moved:
                shutil.move(file_path, os.path.join(base_path, "Others", file))

def organize_folder(path):
    create_folders(path)
    move_files(path)
    print(f"✅ Done organizing files in: {path}")

if __name__ == "__main__":
    folder_path = input("Enter folder path to organize: ")
    if os.path.exists(folder_path):
        organize_folder(folder_path)
    else:
        print("❌ Invalid path. Please check and try again.")
