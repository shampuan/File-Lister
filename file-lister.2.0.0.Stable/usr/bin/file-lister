#!/usr/bin/env python3
import sys
import os
import pathlib

# PySide6 yerine PyQt5 importları
from PyQt5.QtWidgets import (
    QApplication, QWidget, QVBoxLayout, QPushButton,
    QFileDialog, QTextEdit, QMessageBox, QLabel, QFrame,
    QHBoxLayout, QDialog, QSizePolicy
)
from PyQt5.QtGui import (
    QIcon, QPixmap, QDragEnterEvent, QDropEvent, QPalette, QColor, QFont
)
from PyQt5.QtCore import (
    Qt, QSize, QUrl
)

# --- GNOME/Qt Platform Plugin Fix kaldırıldı ---

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
        "developer_label": "Geliştirici",
        
        # Liste Başlıkları
        "list_name": "İsim",
        "list_type": "Tür",
        "list_size": "Boyut"
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
        "developer_label": "Developer",
        
        # Liste Başlıkları
        "list_name": "Name",
        "list_type": "Type",
        "list_size": "Size"
    }
}

def apply_dark_theme(app):
    """Koyu tema için QPalette ayarlarını uygular."""
    
    # Fusion stilini kullanmak, çoğu Qt teması için en iyi koyu mod desteğini sağlar
    app.setStyle("Fusion") 

    dark_mode = QPalette()
    
    # Standart Renkler
    dark_mode.setColor(QPalette.Window, QColor(45, 45, 45))
    dark_mode.setColor(QPalette.WindowText, QColor(200, 200, 200))
    dark_mode.setColor(QPalette.Base, QColor(25, 25, 25))
    dark_mode.setColor(QPalette.AlternateBase, QColor(55, 55, 55))
    dark_mode.setColor(QPalette.Text, QColor(200, 200, 200))
    dark_mode.setColor(QPalette.Button, QColor(50, 50, 50))
    dark_mode.setColor(QPalette.ButtonText, QColor(200, 200, 200))
    dark_mode.setColor(QPalette.BrightText, QColor(255, 0, 0))
    
    # Vurgu Renkleri
    dark_mode.setColor(QPalette.Highlight, QColor(0, 120, 215)) # Mavi Vurgu
    dark_mode.setColor(QPalette.HighlightedText, QColor(255, 255, 255))
    
    # Tooltip Renkleri
    dark_mode.setColor(QPalette.ToolTipBase, QColor(20, 20, 20))
    dark_mode.setColor(QPalette.ToolTipText, QColor(200, 200, 200))

    # Devre dışı (Disabled) öğeler
    dark_mode.setColor(QPalette.Disabled, QPalette.Text, QColor(100, 100, 100))
    dark_mode.setColor(QPalette.Disabled, QPalette.ButtonText, QColor(100, 100, 100))
    
    app.setPalette(dark_mode)


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
            # QPixmap'deki Qt.AspectRatioMode ve Qt.TransformationMode kullanıldı
            pixmap = QPixmap(self.icon_path).scaled(96, 96, Qt.KeepAspectRatio, Qt.SmoothTransformation) 
            logo_label.setPixmap(pixmap)
        else:
            logo_label.setText("LOGO")
        logo_label.setAlignment(Qt.AlignCenter) # Qt.AlignmentFlag kısaltıldı
        main_layout.addWidget(logo_label)
        
        # Bilgi Alanı
        lang = self.current_lang
        info_text = f"""
        <center>
        <b>{TEXTS[lang]["title"]}</b>
        <hr>
        {TEXTS[lang]["version_label"]}: 2.0.0 - PyQt5 & Dark Theme Update<br>
        {TEXTS[lang]["license_label"]}: GNU GPLv3<br>
        {TEXTS[lang]["prog_lang_label"]}: Python3<br>
        {TEXTS[lang]["interface_label"]}: Qt5-PyQt5<br>
        {TEXTS[lang]["developer_label"]}: A. Serhat KILIÇOĞLU (shampuan)<br>
        Github: <a style="color:#0078D4;" href="www.github.com/shampuan">www.github.com/shampuan</a>
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
        info_label.setTextFormat(Qt.RichText) # Qt.TextFormat kısaltıldı
        info_label.setAlignment(Qt.AlignCenter) # Qt.AlignmentFlag kısaltıldı
        # Koyu temada RichText link rengini ayarlamak için CSS eklendi.
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
        
        # Koyu temaya uygun renkler
        self.default_style = ("""
            DropArea { 
                border: 3px dashed #607d8b; /* Koyu Mavi-Gri */
                border-radius: 8px; 
                background-color: #353535; /* Koyu Gri Arka Plan */
            } 
            DropArea QLabel { 
                color: #cfd8dc; /* Açık Mavi-Gri Metin */
                font-weight: bold; 
                font-size: 16px; 
            }
        """)
        self.highlight_style = ("""
            DropArea { 
                border: 3px dashed #455a64; 
                border-radius: 8px; 
                background-color: #454545; 
            } 
            DropArea QLabel { 
                color: #ffffff; 
                font-weight: bold; 
                font-size: 16px; 
            }
        """)
        
        self.text_label = QLabel(TEXTS[self.current_lang]["drag_drop"], self)
        self.text_label.setAlignment(Qt.AlignCenter)
        
        frame_layout = QVBoxLayout(self) 
        frame_layout.addWidget(self.text_label)
        frame_layout.setContentsMargins(0, 0, 0, 0)
        self.setLayout(frame_layout)
        
        self.setFixedSize(580, 80) 
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
            # QPixmap'deki Qt.KeepAspectRatio kullanıldı
            pixmap = QPixmap(self.icon_path).scaled(64, 64, Qt.KeepAspectRatio, Qt.SmoothTransformation)
            self.icon_label.setPixmap(pixmap)
        else:
             self.icon_label.setText("Amblem (İkon) Yüklenemedi")
             
        self.icon_label.setAlignment(Qt.AlignCenter) # Qt.AlignmentFlag kısaltıldı
        self.icon_label.setAcceptDrops(False)
        layout.addWidget(self.icon_label)
        
        # 2. SÜRÜKLE-BIRAK BÖLGESİ
        self.drop_area = DropArea(self.current_lang, self) 
        layout.addWidget(self.drop_area, alignment=Qt.AlignCenter) # Qt.AlignmentFlag kısaltıldı

        # 3. KLASÖR SEÇ BUTONU
        self.select_button = QPushButton(TEXTS[self.current_lang]["select_dir"])
        self.select_button.clicked.connect(self.select_directory)
        layout.addWidget(self.select_button)

        # 4. DİĞER ÖĞELER
        self.text_area = QTextEdit()
        # Ağaç yapısının düzgün görünmesi için monospace font önerilir.
        self.text_area.setFont(QFont("Monospace")) 
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
        self._about_dialog.exec_() # PyQt5'te exec() yerine exec_() kullanılır
    
    # PyQt5'te genel koyu tema ayarlandığı için bu fonksiyon iptal edildi veya basitleştirildi.
    # def changeEvent(self, event):
    #     super().changeEvent(event)
    #     if event.type() == event.Type.PaletteChange:
    #         self.update() 
            
    # GELİŞTİRİLMİŞ ASCII AĞAÇ YAPISI VE FORMATLAMA METOTLARI
    
    def get_file_info(self, full_path, lang):
        """Bir dosya veya dizin için tür ve boyut bilgisini döndürür."""
        if os.path.isdir(full_path):
            ext = "KLASÖR" if lang == LANG_TR else "FOLDER"
            size = "-"
        elif os.path.isfile(full_path):
            ext = os.path.splitext(full_path)[1].lstrip('.') or ("Yok" if lang == LANG_TR else "None")
            try:
                # Boyutu KB/MB/GB cinsinden biçimlendir
                size_bytes = os.path.getsize(full_path)
                
                if size_bytes >= 1024**3: # GB
                    size = f"{size_bytes / 1024**3:.2f} GB"
                elif size_bytes >= 1024**2: # MB
                    size = f"{size_bytes / 1024**2:.2f} MB"
                elif size_bytes >= 1024: # KB
                    size = f"{size_bytes / 1024:.2f} KB"
                else: # Bytes
                    size = f"{size_bytes} B"
                    
            except OSError:
                size = "N/A"
        else:
            ext = "Bilinmeyen" if lang == LANG_TR else "Unknown"
            size = "-"
        return ext, size

    def build_tree_recursive(self, directory, prefix, lines, max_widths, lang):
        """
        Dizin içeriğini özyinelemeli olarak tarar ve ASCII ağaç yapısını oluşturur.
        
        :param directory: Şu anki dizin yolu.
        :param prefix: Ağaç yapısı için önek (örn: "│   " veya "    ").
        :param lines: Çıktı satırlarının eklendiği liste.
        :param max_widths: Sütun genişliklerini dinamik olarak tutan sözlük.
        :param lang: Mevcut dil.
        """
        try:
            # Sadece dosya ve klasörleri al, sırala (klasörler üste)
            entries = os.listdir(directory)
            entries.sort(key=lambda x: (not os.path.isdir(os.path.join(directory, x)), x.lower()))
            
        except Exception as e:
            lines.append((f"{prefix}LİSTELEME HATASI ({directory}): {e}", "Hata", "-"))
            return

        for i, entry in enumerate(entries):
            full_path = os.path.join(directory, entry)
            is_last = (i == len(entries) - 1)
            
            # Ağaç Yapı Sembolleri
            pointer = "└── " if is_last else "├── "
            # Yeni önek, bir sonraki seviyenin dikey çizgisi (pipe) veya boşluk olacak
            new_prefix = prefix + ("    " if is_last else "│   ")
            
            ext, size = self.get_file_info(full_path, lang)
            
            # Ağaç görünümü için metin ve bilgileri hazırla: prefix + pointer + entry
            tree_part = f"{prefix}{pointer}{entry}"
            
            # Dinamik genişlikleri güncelle
            max_widths['name'] = max(max_widths['name'], len(tree_part))
            max_widths['type'] = max(max_widths['type'], len(ext))
            max_widths['size'] = max(max_widths['size'], len(size))
            
            # Satırı geçici olarak ekle (biçimlendirme daha sonra yapılacak)
            lines.append((tree_part, ext, size))

            # Eğer klasörse, özyinelemeli olarak devam et
            if os.path.isdir(full_path) and not entry.startswith('.'):
                self.build_tree_recursive(full_path, new_prefix, lines, max_widths, lang)

    def format_tree_output(self, directory, lines_data, max_widths, lang):
        """Çıktı satırlarını sütun genişliklerine göre biçimlendirir."""
        
        # Başlıkları çevir
        header_name = TEXTS[lang]["list_name"]
        header_type = TEXTS[lang]["list_type"]
        header_size = TEXTS[lang]["list_size"]
        
        # Başlıklar için minimum genişlikleri ayarla
        root_name = os.path.basename(directory)
        name_width = max(max_widths['name'], len(header_name), len(root_name)) + 2 
        type_width = max(max_widths['type'], len(header_type)) + 2
        size_width = max(max_widths['size'], len(header_size)) + 2
        
        # Header'ı oluştur
        header_line = (
            f"{header_name.ljust(name_width)}| "
            f"{header_type.ljust(type_width)}| "
            f"{header_size.ljust(size_width)}"
        )
        separator = "=" * len(header_line)
        
        # Kök Dizin Bilgisi
        root_ext, root_size = self.get_file_info(directory, lang)
        root_line = (
            f"{root_name.ljust(name_width)}| "
            f"{root_ext.ljust(type_width)}| "
            f"{root_size.ljust(size_width)}"
        )
        
        output = [directory, separator, header_line, separator, root_line, separator]
        
        # Veri satırlarını biçimlendir
        for tree_part, ext, size in lines_data:
            line = (
                f"{tree_part.ljust(name_width)}| "
                f"{ext.ljust(type_width)}| "
                f"{size.ljust(size_width)}"
            )
            output.append(line)
            
        return "\n".join(output)


    def list_directory(self, directory):
        """Belirtilen dizini gelişmiş ASCII ağaç görünümüyle listeler."""
        lang = self.current_lang
        
        lines_data = [] 
        max_widths = {'name': len(os.path.basename(directory)), 'type': 0, 'size': 0}

        try:
            # Kök dizin içeriğini listelemeye başla (prefix boş, bu kök dizin altındaki ilk seviye)
            self.build_tree_recursive(directory, "", lines_data, max_widths, lang)

        except Exception as e:
            self.text_area.setPlainText(f"{TEXTS[lang]['dir_read_error']}\n{e}")
            return

        if not lines_data and not any(os.listdir(directory)):
            self.text_area.setPlainText(f"'{directory}' {TEXTS[lang]['no_entries']}")
            return

        # Çıktıyı biçimlendir
        formatted_output = self.format_tree_output(directory, lines_data, max_widths, lang)
        
        self.text_area.setPlainText(formatted_output)

    def select_directory(self):
        # PyQt5'te QFileDialog.getExistingDirectory sadece bir string döndürür.
        directory = QFileDialog.getExistingDirectory(self, TEXTS[self.current_lang]["select_dir"])
        if directory:
            self.list_directory(directory)

    def save_to_txt(self):
        text = self.text_area.toPlainText()
        lang = self.current_lang
        
        if not text.strip():
            QMessageBox.warning(self, TEXTS[lang]["warn_title"], TEXTS[lang]["warn_no_content"])
            return

        # PyQt5'te QFileDialog.getSaveFileName bir tuple döndürür (path, filter)
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
    
    # KOYU TEMAYI UYGULA
    apply_dark_theme(app)
    
    window = DizinListeleyici()
    window.show()
    # PyQt5'te exec() yerine exec_() kullanılır
    sys.exit(app.exec_())
