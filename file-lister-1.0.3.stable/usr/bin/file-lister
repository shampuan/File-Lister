#!/usr/bin/env python3
import sys
import os
import pathlib
from PySide6.QtWidgets import (
    QApplication, QWidget, QVBoxLayout, QPushButton,
    QFileDialog, QTextEdit, QMessageBox, QLabel, QFrame,
    QHBoxLayout, QDialog, QSizePolicy
)
from PySide6.QtGui import (
    QIcon, QPixmap, QDragEnterEvent, QDropEvent
)
from PySide6.QtCore import (
    Qt, QSize, QUrl
)

# --- GNOME/Qt Platform Plugin Fix ---
os.environ['QT_QPA_PLATFORM_PLUGIN_PATH'] = ''
os.environ['QT_PLUGIN_PATH'] = ''
# --- Fix Sonu ---

# Dil Verileri
LANG_TR = "TR"
LANG_EN = "EN"

TEXTS = {
    LANG_TR: {
        # Program Adı Güncellendi
        "title": "File Lister", 
        "drag_drop": "Dizini Buraya Sürükle",
        "select_dir": "Klasör Seç",
        "save_list": "Listeyi TXT olarak kaydet",
        "lang_btn": "Language",
        "about_btn": "Hakkında",
        "warn_title": "Uyarı",
        "warn_dir_only": "Lütfen bir **dizin (klasör)** sürükleyip bırakın.",
        "warn_no_content": "Kaydedilecek içerik yok.",
        "success_title": "Başarılı",
        "success_msg": "Liste kaydedildi.",
        "error_title": "Hata",
        "dir_read_error": "Dizin okuma hatası:",
        "no_entries": "klasöründe dosya ya da klasör yok.",
        
        # Hakkında Menüsü
        "about_box_title": "Dosya Listeleyici Hakkında",
        "about_button_ok": "Tamam",
        "about_description": "Bu program, seçtiğiniz dizinlerin içindekileri listeleyerek size gösterir ve isterseniz size txt çıktı üretir.",
        "about_guarantee": "Bu program kesinlikle hiçbir garanti getirmiyor.",
        "about_copyright": "Copyright © 2025 A. Serhat KILIÇOĞLU",
        "version_label": "Versiyon",
        "license_label": "Lisans",
        "prog_lang_label": "Programlama dili",
        "interface_label": "Arayüz",
        "developer_label": "Geliştirici"
    },
    LANG_EN: {
        # Program Adı Güncellendi
        "title": "File Lister", 
        "drag_drop": "Drag Directory Here",
        "select_dir": "Select Directory",
        "save_list": "Save List as TXT",
        "lang_btn": "Language",
        "about_btn": "About",
        "warn_title": "Warning",
        "warn_dir_only": "Please drag and drop a **directory**.",
        "warn_no_content": "No content to save.",
        "success_title": "Success",
        "success_msg": "List saved successfully.",
        "error_title": "Error",
        "dir_read_error": "Directory reading error:",
        "no_entries": "folder contains no files or folders.",
        
        # Hakkında Menüsü
        "about_box_title": "About File Lister",
        "about_button_ok": "OK",
        "about_description": "This program lists the contents of the directories you select, shows them to you, and generates a TXT output if desired.",
        "about_guarantee": "This program comes with absolutely no warranty.",
        "about_copyright": "Copyright © 2025 A. Serhat KILIÇOĞLU",
        "version_label": "Version",
        "license_label": "License",
        "prog_lang_label": "Programming language",
        "interface_label": "Interface",
        "developer_label": "Developer"
    }
}

class AboutDialog(QDialog):
    def __init__(self, current_lang, icon_path, parent=None):
        super().__init__(parent)
        self.current_lang = current_lang
        self.icon_path = icon_path
        self.setWindowTitle(TEXTS[self.current_lang]["about_box_title"])
        self.setFixedSize(400, 450)
        self.init_ui()

    def init_ui(self):
        main_layout = QVBoxLayout()
        
        # Logo
        logo_label = QLabel()
        if os.path.exists(self.icon_path):
            pixmap = QPixmap(self.icon_path).scaled(96, 96, Qt.AspectRatioMode.KeepAspectRatio, Qt.TransformationMode.SmoothTransformation)
            logo_label.setPixmap(pixmap)
        else:
            logo_label.setText("LOGO")
        logo_label.setAlignment(Qt.AlignmentFlag.AlignCenter)
        main_layout.addWidget(logo_label)
        
        # Bilgi Alanı
        lang = self.current_lang
        info_text = f"""
        <center>
        <b>{TEXTS[lang]["title"]}</b>
        <hr>
        {TEXTS[lang]["version_label"]}: 1.0.3<br>
        {TEXTS[lang]["license_label"]}: GNU GPLv3<br>
        {TEXTS[lang]["prog_lang_label"]}: Python3<br>
        {TEXTS[lang]["interface_label"]}: Qt6-PySide6<br>
        {TEXTS[lang]["developer_label"]}: A. Serhat KILIÇOĞLU (shampuan)<br>
        Github: <a href="www.github.com/shampuan">www.github.com/shampuan</a>
        <br><br>
        <i>{TEXTS[lang]["about_description"]}</i>
        <br><br>
        <b>{TEXTS[lang]["about_guarantee"]}</b>
        <br><br>
        {TEXTS[lang]["about_copyright"]}
        </center>
        """
        info_label = QLabel(info_text)
        info_label.setWordWrap(True)
        info_label.setTextFormat(Qt.TextFormat.RichText)
        info_label.setAlignment(Qt.AlignmentFlag.AlignCenter)
        main_layout.addWidget(info_label)
        
        # Tamam Butonu
        ok_button = QPushButton(TEXTS[lang]["about_button_ok"])
        ok_button.clicked.connect(self.accept)
        main_layout.addWidget(ok_button)

        self.setLayout(main_layout)


class DropArea(QFrame):
    """Sürükle bırak özelliğini destekleyen, arayüzde kalıcı görünümlü QFrame."""
    def __init__(self, current_lang, parent=None):
        super().__init__(parent)
        self.current_lang = current_lang
        self.setAcceptDrops(True)
        
        self.text_label = QLabel(TEXTS[self.current_lang]["drag_drop"], self)
        self.text_label.setAlignment(Qt.AlignmentFlag.AlignCenter)
        
        frame_layout = QVBoxLayout(self) 
        frame_layout.addWidget(self.text_label)
        frame_layout.setContentsMargins(0, 0, 0, 0)
        self.setLayout(frame_layout)
        
        self.setFixedSize(580, 80) 
        
        self.default_style = ("""DropArea { border: 3px dashed #3498db; border-radius: 8px; background-color: transparent; } DropArea QLabel { color: #3498db; font-weight: bold; font-size: 16px; }""")
        self.highlight_style = ("""DropArea { border: 3px dashed #2980b9; border-radius: 8px; background-color: #ebf5fb; } DropArea QLabel { color: #2980b9; font-weight: bold; font-size: 16px; }""")
        self.setStyleSheet(self.default_style)

    def update_language(self, new_lang):
        """Dil değişiminde DropArea yazısını günceller."""
        self.current_lang = new_lang
        self.text_label.setText(TEXTS[self.current_lang]["drag_drop"])

    def dragEnterEvent(self, event: QDragEnterEvent):
        if event.mimeData().hasUrls():
            event.acceptProposedAction()
            self.setStyleSheet(self.highlight_style)
        else:
            event.ignore()

    def dragLeaveEvent(self, event):
        self.setStyleSheet(self.default_style)

    def dropEvent(self, event: QDropEvent):
        self.setStyleSheet(self.default_style) 
        
        urls = event.mimeData().urls()
        if urls:
            path = urls[0].toLocalFile()
            if os.path.isdir(path):
                if self.parentWidget() and hasattr(self.parentWidget(), 'list_directory'):
                    self.parentWidget().list_directory(path)
                event.acceptProposedAction()
            else:
                QMessageBox.warning(self.parentWidget(), TEXTS[self.current_lang]["warn_title"], TEXTS[self.current_lang]["warn_dir_only"])
                event.ignore()
        else:
            event.ignore()


class DizinListeleyici(QWidget):
    def __init__(self):
        super().__init__()
        
        # İKON YOLU BULMA MANTIĞI: filelister.png ve /usr/share/filelister/
        ICON_FILENAME = "filelister.png"
        LOCAL_PATH = pathlib.Path(ICON_FILENAME)
        # Program adı "filelister" olarak güncellendiği için sistem yolu da değişti
        SYSTEM_PATH = pathlib.Path("/usr/share/filelister") / ICON_FILENAME
        
        if LOCAL_PATH.exists():
            self.icon_path = str(LOCAL_PATH)
        elif SYSTEM_PATH.exists():
            self.icon_path = str(SYSTEM_PATH)
        else:
            self.icon_path = "" # İkon bulunamazsa boş bırakılır
            
        self.current_lang = LANG_EN # VARSAYILAN DİL İNGİLİZCE
        
        if self.icon_path:
            self.setWindowIcon(QIcon(self.icon_path))
        
        self.setWindowTitle(TEXTS[self.current_lang]["title"])
        self.setGeometry(100, 100, 600, 550) 

        self.setup_ui()
        
    
    def setup_ui(self):
        layout = QVBoxLayout()
        
        # 1. AMBLEM (İkon)
        self.icon_label = QLabel()
        if self.icon_path:
            pixmap = QPixmap(self.icon_path).scaled(64, 64, Qt.AspectRatioMode.KeepAspectRatio, Qt.TransformationMode.SmoothTransformation)
            self.icon_label.setPixmap(pixmap)
        else:
             self.icon_label.setText("Amblem (İkon) Yüklenemedi")
             
        self.icon_label.setAlignment(Qt.AlignmentFlag.AlignCenter)
        self.icon_label.setAcceptDrops(False)
        layout.addWidget(self.icon_label)
        
        # 2. SÜRÜKLE-BIRAK BÖLGESİ
        self.drop_area = DropArea(self.current_lang, self) 
        layout.addWidget(self.drop_area, alignment=Qt.AlignmentFlag.AlignCenter) 

        # 3. KLASÖR SEÇ BUTONU
        self.select_button = QPushButton(TEXTS[self.current_lang]["select_dir"])
        self.select_button.clicked.connect(self.select_directory)
        layout.addWidget(self.select_button)

        # 4. DİĞER ÖĞELER
        self.text_area = QTextEdit()
        layout.addWidget(self.text_area)

        # 5. ALT BUTONLAR (Save, Language, About)
        self.save_button = QPushButton(TEXTS[self.current_lang]["save_list"])
        self.save_button.clicked.connect(self.save_to_txt)
        layout.addWidget(self.save_button)
        
        # Language ve Hakkında butonları için yatay layout
        bottom_layout = QHBoxLayout()
        
        self.lang_button = QPushButton(TEXTS[self.current_lang]["lang_btn"])
        self.lang_button.clicked.connect(self.toggle_language)
        bottom_layout.addWidget(self.lang_button)
        
        self.about_button = QPushButton(TEXTS[self.current_lang]["about_btn"])
        self.about_button.clicked.connect(self.show_about_dialog)
        bottom_layout.addWidget(self.about_button)
        
        layout.addLayout(bottom_layout)

        self.setLayout(layout)
    
    def toggle_language(self):
        """Dili İngilizce ve Türkçe arasında değiştirir ve tüm arayüzü günceller."""
        self.current_lang = LANG_TR if self.current_lang == LANG_EN else LANG_EN
        self.update_ui_text()

    def update_ui_text(self):
        """Tüm widget'ların metinlerini yeni dile göre günceller."""
        
        lang = self.current_lang
        
        # 1. Pencere Başlığı
        self.setWindowTitle(TEXTS[lang]["title"])
        
        # 2. DropArea Metni
        self.drop_area.update_language(lang)
        
        # 3. Buton Metinleri
        self.select_button.setText(TEXTS[lang]["select_dir"])
        self.save_button.setText(TEXTS[lang]["save_list"])
        self.lang_button.setText(TEXTS[lang]["lang_btn"])
        self.about_button.setText(TEXTS[lang]["about_btn"])
        
        # 4. Hakkında menüsü açıksa, onu da yeni dil ile yeniden oluşturarak güncelle
        if hasattr(self, '_about_dialog') and self._about_dialog.isVisible():
            self._about_dialog.close()
            self.show_about_dialog()

    def show_about_dialog(self):
        """Hakkında penceresini açar."""
        self._about_dialog = AboutDialog(self.current_lang, self.icon_path, self)
        self._about_dialog.exec()
    
    # Tema güncellemeleri PySide6'da kaldı
    def changeEvent(self, event):
        super().changeEvent(event)
        if event.type() == event.Type.PaletteChange:
            QApplication.setPalette(QApplication.palette())
            QApplication.setStyle(QApplication.style().name()) 
            for child in self.findChildren(QWidget):
                child.setPalette(QApplication.palette())
                child.update()
            self.update() 

    def list_directory(self, directory):
        """Belirtilen dizini listeler."""
        entries = []
        lang = self.current_lang
        try:
            for entry in os.listdir(directory):
                full_path = os.path.join(directory, entry)
                if os.path.isdir(full_path):
                    name = entry + '/'
                    ext = "KLASÖR" if lang == LANG_TR else "FOLDER" 
                    size = "-"
                elif os.path.isfile(full_path):
                    name = entry
                    ext = os.path.splitext(entry)[1].lstrip('.') or ("Yok" if lang == LANG_TR else "None")
                    size = f"{os.path.getsize(full_path) / 1024:.2f} KB"
                else:
                    continue

                entries.append((name, ext, size))
        except Exception as e:
            self.text_area.setPlainText(f"{TEXTS[lang]['dir_read_error']}\n{e}")
            return

        if not entries:
            self.text_area.setPlainText(f"'{directory}' {TEXTS[lang]['no_entries']}")
            return

        # Listeleme başlıklarının çevirisi
        header_name = ("İsim" if lang == LANG_TR else "Name").ljust(30)
        header_type = ("Tür" if lang == LANG_TR else "Type").ljust(10)
        header_size = ("Boyut" if lang == LANG_TR else "Size")
        header = (header_name + "| " + header_type + "| " + header_size)
        separator = "-" * len(header)

        name_width = max(len(name) for name, _, _ in entries + [("İsim", "", "")]) + 2
        ext_width = max(len(ext) for _, ext, _ in entries + [("", "Tür", "")]) + 2

        lines = [header, separator]
        for name, ext, size in sorted(entries):
            line = f"{name.ljust(name_width)}| {ext.ljust(ext_width)}| {size}"
            lines.append(line)

        self.text_area.setPlainText("\n".join(lines))

    def select_directory(self):
        directory = QFileDialog.getExistingDirectory(self, TEXTS[self.current_lang]["select_dir"])
        if directory:
            self.list_directory(directory)

    def save_to_txt(self):
        text = self.text_area.toPlainText()
        lang = self.current_lang
        
        if not text.strip():
            QMessageBox.warning(self, TEXTS[lang]["warn_title"], TEXTS[lang]["warn_no_content"])
            return

        path, _ = QFileDialog.getSaveFileName(
            self, TEXTS[lang]["save_list"], "dosya_listesi.txt", "Text Files (*.txt)"
        )
        if path:
            try:
                with open(path, "w", encoding="utf-8") as f:
                    f.write(text)
                QMessageBox.information(self, TEXTS[lang]["success_title"], TEXTS[lang]["success_msg"])
            except Exception as e:
                QMessageBox.critical(self, TEXTS[lang]["error_title"], f"{TEXTS[lang]['error_title']}:\n{e}")

if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = DizinListeleyici()
    window.show()
    sys.exit(app.exec())
