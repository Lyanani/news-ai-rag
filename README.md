# news-ai-rag

RAG News Analyzer

Sistem Analisis Berita berbasis Retrieval-Augmented Generation (RAG)
Menggabungkan kemampuan pencarian semantik, basis data vektor, dan LLM untuk menjawab pertanyaan berdasarkan berita yang dimasukkan pengguna.

Komponen RAG

Komponen	Teknologi yang Digunakan
Embedding	Google Gemini API (gemini-embedding-001)
Vector Database	ChromaDB (persistent storage)
Retrieval	Semantic search + reranking berdasarkan jarak kemiripan
LLM (Generasi Jawaban)	Groq API (llama-3.3-70b-versatile) dengan fallback ke Gemini
Fitur Utama

5 metode input berita:

Teks manual (copy-paste)
URL berita biasa (scraping otomatis dengan 4 strategi: RSS → requests → cloudscraper → curl_cffi)
URL X/Twitter (via FixTweet API)
Upload file (.txt, .pdf, .docx)
Pencarian kata kunci (via GNews API)
Antarmuka ganda:

CLI (chat_news()) untuk interaksi terminal
Gradio Web UI untuk antarmuka grafis (link publik)
Pencegahan duplikat: hash MD5 konten

Prioritas berita terakhir: sistem langsung menjawab berdasarkan berita yang baru diinput

Fallback API: jika Groq bermasalah (error 503), otomatis beralih ke Gemini

Prasyarat

Python 3.10+
Akun Kaggle (untuk menjalankan notebook) atau lingkungan lokal dengan akses internet
API keys (simpan sebagai Kaggle Secrets atau environment variable):
Google Gemini API → GOOGLE_API_KEY
Groq API → GROQ_API_KEY
GNews API (opsional) → GNEWS_API_KEY
Cara Menjalankan

1. Clone repository
2. Install dependencies
pip install -U google-genai chromadb beautifulsoup4 requests tenacity groq cloudscraper nest-asyncio PyPDF2 python-docx feedparser gradio

🔧 Siapkan API Keys

Di Kaggle: buat secret dengan nama:

GOOGLE_API_KEY
GROQ_API_KEY
GNEWS_API_KEY (opsional)
Di lokal: atur environment variable atau gunakan file .env (jangan hardcode di kode).

4. Jalankan notebook

Buka final-project-expert-system-kelompok3.ipynb di Kaggle/Colab, lalu jalankan semua sel secara berurutan (Run All).

5. Gunakan antarmuka

CLI: jalankan sel yang berisi chat_news().
Gradio: jalankan sel yang berisi demo.launch(share=True).
Akan muncul link publik seperti https://xxxx.gradio.live.
Struktur File (Notebook)

Sel	Isi
1	Instalasi library
2	Import semua modul
3–5	Setup API keys (Gemini, Groq, GNews)
6	Kelas GeminiEmbeddingFunction (embedding)
7	Fungsi chunk_text (dengan overlap)
8–9	Fungsi scraping & tweet
10	Pencarian GNews
11	Inisialisasi ChromaDB & add_news_to_db
12–13	Retrieval & generate answer (Groq + fallback)
14–15	Menu input & interactive search
16–17	Fungsi CLI & Gradio helper
18–20	Antarmuka Gradio web
Contoh Penggunaan (Gradio)

Setelah menjalankan sel Gradio (demo.launch(share=True)), buka link publik (misal https://xxxx.gradio.live). Antarmuka akan menampilkan beberapa tab.

Tab 1: Tanya Berita

Ketik pertanyaan di kotak chat, contoh:
Apa keputusan BI tentang suku bunga?
Sistem akan menjawab berdasarkan berita terakhir yang ditambahkan.
Tab 2: Tambah Berita

Teks Manual: isi judul, teks berita, dan sumber → klik Simpan.
URL Berita: masukkan URL artikel → klik Ambil & Simpan.
X/Twitter: masukkan URL tweet → klik Ambil & Simpan.
Upload File: pilih file .txt, .pdf, atau .docx → klik Simpan dari File.
Cari Kata Kunci: masukkan kata kunci (misal ekonomi) → klik Cari → pilih berita dari dropdown → klik Ambil & Simpan.
Setelah berita tersimpan, langsung beralih ke tab Tanya Berita untuk mengajukan pertanyaan.

Catatan Teknis

Chunking: ukuran 900 karakter dengan overlap 150 untuk menjaga konteks agar tidak terputus.
Retrieval: mengambil 5 chunk teratas, lalu reranking (urut ulang) ambil 3 terdekat berdasarkan jarak kemiripan.
Scraping: urutan strategi yang digunakan:
RSS feed (jika tersedia)
requests biasa (dengan headers lengkap seperti browser)
cloudscraper (untuk melewati proteksi Cloudflare)
curl_cffi (meniru fingerprint browser Chrome/Safari)
Hash duplikat: setiap konten di-hash MD5, disimpan di metadata Chroma, dicek sebelum insert.
Prioritas berita terakhir: variabel global last_added_content memastikan sistem menjawab dari berita yang baru diinput tanpa harus mencari database.
Kontributor

Kelompok 3 – Final Project Expert System

Angginaloy, Syalom Mauren
Lakoy, Gyssella Viola Visya
Lonteng, Gea Paulina
Nani, Natalya De Chantal Putri
Walukow, Nadine Kristania
