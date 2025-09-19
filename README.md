<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Orang Kepercayaan?", "id": "Tangan Kanan." },
  { "en": "Suka Mengambil Barang Orang?", "id": "Panjang Tangan." },
  { "en": "Hadiah Atau Oleh-oleh?", "id": "Buah Tangan." },
  { "en": "Anak Kandung Kesayangan?", "id": "Buah Hati." },
  { "en": "Topik Perbincangan Hangat?", "id": "Buah Bibir." },
  { "en": "Sedang Populer Atau Terkenal?", "id": "Naik Daun." },
  { "en": "Mengalami Kebangkrutan?", "id": "Gulung Tikar." },
  { "en": "Tidak Punya Rasa Malu?", "id": "Muka Tembok." },
  { "en": "Menunjukkan Kemampuan Hebat?", "id": "Unjuk Gigi." },
  { "en": "Bekerja Keras Sekali?", "id": "Banting Tulang." },
  { "en": "Menghindari Tanggung Jawab?", "id": "Cuci Tangan." },
  { "en": "Sangat Pandai Atau Cerdas?", "id": "Otak Encer." },
  { "en": "Berita Yang Belum Pasti?", "id": "Kabar Angin." },
  { "en": "Suka Memberi Bantuan?", "id": "Ringan Tangan." },
  { "en": "Uang Sogokan?", "id": "Uang Pelicin." },
  { "en": "Sombong Atau Congkak?", "id": "Besar Kepala." },
  { "en": "Sabar Menghadapi Masalah?", "id": "Kepala Dingin." },
  { "en": "Membuat Orang Lain Bertengkar?", "id": "Adu Domba." },
  { "en": "Menyerah Sepenuhnya?", "id": "Angkat Tangan." },
  { "en": "Meninggal Dunia?", "id": "Tutup Usia." },
  { "en": "Merasa Sangat Sakit Hati?", "id": "Makan Hati." },
  { "en": "Pemberi Pinjaman Bunga Tinggi?", "id": "Lintah Darat." },
  { "en": "Membawa Ke Pengadilan?", "id": "Meja Hijau." },
  { "en": "Orang Yang Disalahkan?", "id": "Kambing Hitam." },
  { "en": "Anak Paling Disayangi?", "id": "Anak Emas." },
  { "en": "Keturunan Bangsawan?", "id": "Darah Biru." },
  { "en": "Mulai Memberi Pendapat?", "id": "Angkat Bicara." },
  { "en": "Pergi Meninggalkan Suatu Tempat?", "id": "Angkat Kaki." },
  { "en": "Orang Yang Munafik?", "id": "Bermuka Dua." },
  { "en": "Anak Hasil Adopsi?", "id": "Anak Pungut." },
  { "en": "Orang Baru Belum Berpengalaman?", "id": "Bau Kencur." },
  { "en": "Tulisan Yang Sangat Jelek?", "id": "Cakar Ayam." },
  { "en": "Orang Tidak Berguna?", "id": "Sampah Masyarakat." },
  { "en": "Perkataan Manis Untuk Merayu?", "id": "Mulut Manis." },
  { "en": "Kekasih Tercinta?", "id": "Jantung Hati." },
  { "en": "Berpendirian Sangat Kuat?", "id": "Hati Baja." },
  { "en": "Tidak Memiliki Perasaan?", "id": "Hati Batu." },
  { "en": "Bekerja Keras Mencari Nafkah?", "id": "Peras Keringat." },
  { "en": "Suka Berkata Bohong?", "id": "Lidah Bercabang." },
  { "en": "Memimpin Secara Otoriter?", "id": "Tangan Besi." },
  { "en": "Tidak Peduli Lingkungan Sekitar?", "id": "Tutup Mata." },
  { "en": "Halangan Atau Rintangan?", "id": "Batu Sandungan." },
  { "en": "Sengaja Memancing Keributan?", "id": "Cari Gara-gara." },
  { "en": "Tangisan Yang Pura-pura?", "id": "Air Mata Buaya." },
  { "en": "Suka Membocorkan Rahasia?", "id": "Mulut Ember." },
  { "en": "Wawasan Sangat Terbatas?", "id": "Katak Bawah Tempurung." },
  { "en": "Tidak Bisa Membaca Tulis?", "id": "Buta Huruf." },
  { "en": "Mumpung Mendapat Kesempatan?", "id": "Aji Mumpung." },
  { "en": "Pemain Terbaik Dalam Tim?", "id": "Bintang Lapangan." },
  { "en": "Berpikir Keras Mencari Solusi?", "id": "Putar Otak." },
  { "en": "Lelaki Suka Menggoda Wanita?", "id": "Mata Keranjang." },
  { "en": "Sangat Ingin Dihormati?", "id": "Gila Hormat." },
  { "en": "Sangat Serakah Terhadap Harta?", "id": "Mata Duitan." },
  { "en": "Merasa Takut Atau Cemas?", "id": "Kecil Hati." },
  { "en": "Tidak Sombong?", "id": "Rendam Hati." },
  { "en": "Angkuh Dan Meremehkan?", "id": "Tinggi Hati." },
  { "en": "Sangat Ikhlas Dan Sabar?", "id": "Lapang Dada." },
  { "en": "Mencari Perhatian Dari Atasan?", "id": "Cari Muka." },
  { "en": "Sangat Berhasil Melakukan Sesuatu?", "id": "Tangan Dingin." },
  { "en": "Penopang Utama Keluarga?", "id": "Tulang Punggung." },
  { "en": "Orang Yang Lemah Fisik?", "id": "Tulang Lunak." },
  { "en": "Sangat Bodoh?", "id": "Otak Udang." },
  { "en": "Pergi Tanpa Pamit?", "id": "Hilang Akal." },
  { "en": "Pemberani Sekali?", "id": "Darah Pahlawan." },
  { "en": "Keturunan Rakyat Biasa?", "id": "Darah Rakyat." },
  { "en": "Menjadi Sangat Marah?", "id": "Naik Pitam." },
  { "en": "Bertindak Nekat Tanpa Berpikir?", "id": "Gelap Mata." },
  { "en": "Berbicara Terus Terang?", "id": "Buka Kartu." },
  { "en": "Tidak Punya Uang Sama Sekali?", "id": "Kantong Kering." },
  { "en": "Mendapat Keuntungan Besar?", "id": "Panen Besar." },
  { "en": "Mendapat Rezeki Tak Terduga?", "id": "Durian Runtuh." },
  { "en": "Mengalami Nasib Buruk?", "id": "Apes Nian." },
  { "en": "Orang Yang Suka Berpindah?", "id": "Kutu Loncat." },
  { "en": "Rahasia Yang Terbongkar?", "id": "Pecah Telur." },
  { "en": "Sangat Dibenci?", "id": "Musuh Bebuyutan." },
  { "en": "Masih Ada Ikatan Keluarga?", "id": "Ada Darah." },
  { "en": "Mendamaikan Pihak Bertikai?", "id": "Juru Damai." },
  { "en": "Menyimpan Perasaan Dendam?", "id": "Dendam Kesumat." },
  { "en": "Sangat Kecewa Dan Menyerah?", "id": "Gigit Jari." },
  { "en": "Sangat Suka Tidur?", "id": "Tukang Tidur." },
  { "en": "Uang Palsu?", "id": "Uang Kertas." },
  { "en": "Pergi Merantau?", "id": "Mencari Makan." },
  { "en": "Menjadi Gila?", "id": "Gila Bayangan." },
  { "en": "Sangat Sayang?", "id": "Sayang Sekali." },
  { "en": "Menjadi Tumpuan Harapan?", "id": "Gantungan Hidup." },
  { "en": "Sudah Pasti Akan Terjadi?", "id": "Pasti Terjadi." },
  { "en": "Mengambil Jalan Pintas?", "id": "Jalan Tikus." },
  { "en": "Tidak Ada Jalan Keluar?", "id": "Jalan Buntu." },
  { "en": "Perdebatan Sengit?", "id": "Adu Mulut." },
  { "en": "Pertengkaran Fisik?", "id": "Adu Jotos." },
  { "en": "Sangat Pandai Bersilat Lidah?", "id": "Jago Kicau." },
  { "en": "Suka Berkhayal?", "id": "Mimpi Disiang Bolong." },
  { "en": "Orang Yang Membosankan?", "id": "Garing Sekali." },
  { "en": "Sesuatu Yang Sangat Sulit?", "id": "Pekerjaan Rumah." },
  { "en": "Berwajah Pucat Karena Takut?", "id": "Muka Pucat Pasi." },
  { "en": "Menjadi Pembicaraan Dimana-mana?", "id": "Buah Bibir." },
  { "en": "Berita Bohong Belaka?", "id": "Isapan Jempol." },
  { "en": "Sangat Pelit Sekali?", "id": "Kikir Besi." },
  { "en": "Orang Yang Banyak Bicara?", "id": "Tong Kosong." },
  { "en": "Menyembunyikan Sesuatu?", "id": "Ada Udang." },
  { "en": "Sangat Banyak Masalah?", "id": "Seribu Satu Malam." },
  { "en": "Sangat Cepat Marah?", "id": "Sumbu Pendek." },
  { "en": "Sangat Sabar?", "id": "Sumbu Panjang." },
  { "en": "Tidak Memihak Siapapun?", "id": "Duduk Di Tengah." },
  { "en": "Sangat Fanatik?", "id": "Buta Tuli." },
  { "en": "Orang Yang Keras Hati?", "id": "Hati Keras." },
  { "en": "Mudah Memaafkan?", "id": "Hati Lembut." },
  { "en": "Nasihat Yang Sia-sia?", "id": "Garam Di Laut." },
  { "en": "Sangat Cocok Dan Serasi?", "id": "Tumbu Dapatkan Tutup." },
  { "en": "Melakukan Pekerjaan Sia-sia?", "id": "Menegakkan Benang." },
  { "en": "Berita Yang Dibesar-besarkan?", "id": "Api Jadi Bara." },
  { "en": "Rahasia Umum?", "id": "Rahasia Dapur." },
  { "en": "Orang Yang Sangat Kaya?", "id": "Raja Uang." },
  { "en": "Atlet Terbaik?", "id": "Raja Olahraga." },
  { "en": "Penjahat Kelas Kakap?", "id": "Gembong Narkoba." },
  { "en": "Tempat Prostitusi?", "id": "Lembah Hitam." },
  { "en": "Masa Tua?", "id": "Senja Kala." },
  { "en": "Masa Muda?", "id": "Musim Semi." },
  { "en": "Hukuman Mati?", "id": "Tiang Gantungan." },
  { "en": "Sangat Berani Mati?", "id": "Taruhan Nyawa." },
  { "en": "Orang Yang Sangat Setia?", "id": "Anjing Penjaga." },
  { "en": "Pengkhianat Bangsa?", "id": "Anjing Belanda." },
  { "en": "Orang Yang Lemah?", "id": "Domba Hitam." },
  { "en": "Anak Yang Nakal?", "id": "Kuda Liar." },
  { "en": "Pekerja Keras?", "id": "Kuda Beban." },
  { "en": "Orang Yang Lincah?", "id": "Belut Pulang." },
  { "en": "Orang Yang Suka Menipu?", "id": "Tukang Sulap." },
  { "en": "Sangat Terang?", "id": "Siang Bolong." },
  { "en": "Sangat Gelap?", "id": "Malam Kelam." },
  { "en": "Wajah Yang Murung?", "id": "Awan Kelabu." },
  { "en": "Sangat Bahagia?", "id": "Langit Cerah." },
  { "en": "Sangat Sulit?", "id": "Mendaki Gunung." },
  { "en": "Sangat Mudah?", "id": "Turun Gunung." },
  { "en": "Memulai Sesuatu Yang Baru?", "id": "Buka Lembaran." },
  { "en": "Mengakhiri Sesuatu?", "id": "Tutup Buku." },
  { "en": "Sangat Jujur?", "id": "Putih Hati." },
  { "en": "Sangat Jahat?", "id": "Hitam Hati." },
  { "en": "Sangat Berbakat?", "id": "Tangan Emas." },
  { "en": "Suka Merusak?", "id": "Tangan Jahil." },
  { "en": "Tidak Mendapat Apa-apa?", "id": "Tangan Hampa." },
  { "en": "Memberi Tanpa Pamrih?", "id": "Tangan Kasih." },
  { "en": "Sangat Suka Berdebat?", "id": "Debat Kusir." },
  { "en": "Kemenangan Yang Mudah?", "id": "Menang Angin." },
  { "en": "Kekalahan Yang Telak?", "id": "Kalah Arang." },
  { "en": "Menjadi Pemenang?", "id": "Angkat Piala." },
  { "en": "Menjadi Pecundang?", "id": "Gigit Jari." },
  { "en": "Sangat Terkenal?", "id": "Papan Atas." },
  { "en": "Tidak Dikenal?", "id": "Kelas Bawah." },
  { "en": "Sangat Mewah?", "id": "Kelas Kakap." },
  { "en": "Sangat Sederhana?", "id": "Kelas Teri." },
  { "en": "Sangat Berkuasa?", "id": "Tangan Kanan." },
  { "en": "Tidak Punya Kekuasaan?", "id": "Rakyat Jelata." },
  { "en": "Orang Yang Diperalat?", "id": "Bidak Catur." },
  { "en": "Orang Yang Mengatur?", "id": "Dalang Pertunjukan." },
  { "en": "Sesuatu Yang Disembunyikan?", "id": "Gunung Es." },
  { "en": "Masalah Kecil Jadi Besar?", "id": "Bola Salju." },
  { "en": "Sangat Cepat Menyebar?", "id": "Efek Domino." },
  { "en": "Reaksi Berantai?", "id": "Reaksi Berantai." },
  { "en": "Saling Bergantung?", "id": "Simbiosis Mutualisme." },
  { "en": "Saling Merugikan?", "id": "Simbiosis Parasitisme." },
  { "en": "Satu Untung Satu Rugi?", "id": "Simbiosis Komensalisme." },
  { "en": "Mencari Jalan Tengah?", "id": "Win-win Solution." },
  { "en": "Tidak Ada Pemenang?", "id": "Lose-lose Situation." },
  { "en": "Harus Ada Yang Kalah?", "id": "Zero Sum Game." },
  { "en": "Sama-sama Untung?", "id": "Positive Sum Game." },
  { "en": "Menghadapi Dilema?", "id": "Buah Simalakama." },
  { "en": "Pilihan Yang Sulit?", "id": "Pilihan Sulit." },
  { "en": "Tidak Ada Pilihan?", "id": "Tidak Ada Pilihan." },
  { "en": "Banyak Jalan?", "id": "Banyak Jalan." },
  { "en": "Satu-satunya Jalan?", "id": "Satu-satunya Jalan." },
  { "en": "Harapan Terakhir?", "id": "Harapan Terakhir." },
  { "en": "Kesempatan Terakhir?", "id": "Kesempatan Terakhir." },
  { "en": "Momen Penentuan?", "id": "Momen Krusial." },
  { "en": "Titik Balik Kehidupan?", "id": "Titik Balik." },
  { "en": "Awal Yang Baru?", "id": "Awal Yang Baru." },
  { "en": "Akhir Dari Segalanya?", "id": "Akhir Dunia." },
  { "en": "Babak Baru?", "id": "Babak Baru." },
  { "en": "Tirai Telah Ditutup?", "id": "Tutup Tirai." },
  { "en": "Pertunjukan Harus Berlanjut?", "id": "Show Must Go On." },
  { "en": "Di Balik Layar?", "id": "Belakang Panggung." },
  { "en": "Menjadi Bintang Utama?", "id": "Pemeran Utama." },
  { "en": "Menjadi Figuran?", "id": "Pemeran Pembantu." },
  { "en": "Hidup Ini Panggung?", "id": "Panggung Sandiwara." },
  { "en": "Menjalani Peran?", "id": "Jalani Peran." },
  { "en": "Menjadi Sutradara?", "id": "Sutradara Kehidupan." },
  { "en": "Menulis Naskah Sendiri?", "id": "Penulis Naskah." },
  { "en": "Takdir Telah Ditulis?", "id": "Garis Takdir." },
  { "en": "Mengubah Takdir?", "id": "Ubah Takdir." },
  { "en": "Menerima Nasib?", "id": "Terima Nasib." },
  { "en": "Melawan Nasib?", "id": "Lawan Nasib." },
  { "en": "Roda Nasib Berputar?", "id": "Roda Kehidupan." },
  { "en": "Selalu Ada Harapan?", "id": "Matahari Terbit." },
  { "en": "Keputusasaan Total?", "id": "Malam Tanpa Bintang." },
  { "en": "Orang Yang Selalu Gagal?", "id": "Tukang Gagal." },
  { "en": "Orang Yang Selalu Sukses?", "id": "Tangan Emas." },
  { "en": "Menjadi Sangat Terkenal?", "id": "Meroket Namanya." },
  { "en": "Kehilangan Popularitas?", "id": "Turun Pamor." },
  { "en": "Puncak Karir?", "id": "Puncak Kejayaan." },
  { "en": "Akhir Karir?", "id": "Gantung Sepatu." },
  { "en": "Memulai Karir Dari Bawah?", "id": "Mulai Dari Nol." },
  { "en": "Mencapai Sukses Instan?", "id": "Jalan Tol." },
  { "en": "Bekerja Tanpa Lelah?", "id": "Keringat Dingin." },
  { "en": "Bekerja Santai?", "id": "Kerja Ringan." },
  { "en": "Penghasilan Sangat Besar?", "id": "Gaji Buta." },
  { "en": "Penghasilan Sangat Kecil?", "id": "Gaji UMR." },
  { "en": "Mencari Uang Tambahan?", "id": "Kerja Sampingan." },
  { "en": "Pekerjaan Utama?", "id": "Kerja Pokok." },
  { "en": "Sangat Sibuk Bekerja?", "id": "Tenggelam Dalam Kerja." },
  { "en": "Sangat Malas Bekerja?", "id": "Makan Gaji Buta." },
  { "en": "Suka Menunda Pekerjaan?", "id": "Jam Karet." },
  { "en": "Selalu Tepat Waktu?", "id": "Tepat Waktu." },
  { "en": "Sangat Efisien?", "id": "Cepat Dan Tepat." },
  { "en": "Sangat Tidak Efisien?", "id": "Lambat Dan Lelet." },
  { "en": "Kualitas Kerja Terbaik?", "id": "Hasil Jempolan." },
  { "en": "Kualitas Kerja Buruk?", "id": "Hasil Amburadul." },
  { "en": "Sangat Teliti Dan Detail?", "id": "Mata Elang." },
  { "en": "Sangat Ceroboh?", "id": "Tangan Gatal." },
  { "en": "Sangat Kreatif?", "id": "Otak Kanan." },
  { "en": "Sangat Analitis?", "id": "Otak Kiri." },
  { "en": "Sangat Inovatif?", "id": "Penuh Terobosan." },
  { "en": "Sangat Konservatif?", "id": "Cara Lama." },
  { "en": "Berani Mengambil Keputusan?", "id": "Tegas Sekali." },
  { "en": "Selalu Ragu-ragu?", "id": "Plin-plan." },
  { "en": "Sangat Bertanggung Jawab?", "id": "Tanggung Jawab Penuh." },
  { "en": "Tidak Bertanggung Jawab?", "id": "Lepas Tangan." },
  { "en": "Sangat Bisa Dipercaya?", "id": "Bisa Dipegang." },
  { "en": "Tidak Bisa Dipercaya?", "id": "Mulut Manis." },
  { "en": "Selalu Menepati Janji?", "id": "Satu Kata." },
  { "en": "Selalu Ingkar Janji?", "id": "Lidah Bercabang." },
  { "en": "Sangat Setia Kawan?", "id": "Solidaritas Tinggi." },
  { "en": "Suka Mengkhianati Teman?", "id": "Tusuk Dari Belakang." },
  { "en": "Sangat Baik Kepada Semua?", "id": "Baik Hati." },
  { "en": "Hanya Baik Jika Ada Mau?", "id": "Ada Udang." },
  { "en": "Sangat Tulus Membantu?", "id": "Tulus Ikhlas." },
  { "en": "Membantu Dengan Pamrih?", "id": "Ada Maunya." },
  { "en": "Sangat Murah Senyum?", "id": "Ramah Tamah." },
  { "en": "Selalu Berwajah Cemberut?", "id": "Muka Masam." },
  { "en": "Sangat Pandai Bergaul?", "id": "Supel Sekali." },
  { "en": "Sangat Sulit Bergaul?", "id": "Kuper Sekali." },
  { "en": "Sangat Populer Di Kalangannya?", "id": "Anak Gaul." },
  { "en": "Sangat Tertutup?", "id": "Introvert." },
  { "en": "Sangat Terbuka?", "id": "Ekstrovert." },
  { "en": "Pusat Perhatian?", "id": "Bintang Pesta." },
  { "en": "Suka Menyendiri?", "id": "Pojok Ruangan." },
  { "en": "Sangat Suka Bergosip?", "id": "Bibir Gatal." },
  { "en": "Tidak Suka Ikut Campur?", "id": "Tutup Telinga." },
  { "en": "Selalu Ingin Tahu?", "id": "Kepo Sekali." },
  { "en": "Tidak Mau Tahu?", "id": "Masa Bodoh." },
  { "en": "Sangat Peduli?", "id": "Hati Mulia." },
  { "en": "Sangat Egois?", "id": "Mikir Diri Sendiri." },
  { "en": "Sangat Mengutamakan Keluarga?", "id": "Family Man." },
  { "en": "Sangat Gila Kerja?", "id": "Workaholic." },
  { "en": "Sangat Mencintai Alam?", "id": "Anak Alam." },
  { "en": "Sangat Suka Teknologi?", "id": "Anak Gadget." },
  { "en": "Sangat Religius?", "id": "Ahli Ibadah." },
  { "en": "Tidak Percaya Tuhan?", "id": "Ateis." },
  { "en": "Sangat Taat Aturan?", "id": "Anak Baik." },
  { "en": "Suka Melanggar Aturan?", "id": "Anak Nakal." },
  { "en": "Sangat Berani Mengambil Risiko?", "id": "Nekat Sekali." },
  { "en": "Selalu Bermain Aman?", "id": "Cari Aman." },
  { "en": "Sangat Optimis Menghadapi Hidup?", "id": "Gelas Setengah Penuh." },
  { "en": "Sangat Pesimis?", "id": "Gelas Setengah Kosong." },
  { "en": "Selalu Melihat Peluang?", "id": "Mata Jeli." },
  { "en": "Selalu Melihat Masalah?", "id": "Tukang Protes." },
  { "en": "Selalu Mencari Solusi?", "id": "Problem Solver." },
  { "en": "Selalu Menjadi Bagian Masalah?", "id": "Biang Kerok." },
  { "en": "Selalu Membawa Kedamaian?", "id": "Pembawa Damai." },
  { "en": "Selalu Membuat Keributan?", "id": "Pembuat Onar." },
  { "en": "Sangat Bijaksana?", "id": "Kepala Dingin." },
  { "en": "Sangat Gegabah?", "id": "Kepala Panas." },
  { "en": "Sangat Dewasa Dalam Berpikir?", "id": "Pikiran Matang." },
  { "en": "Sangat Kekanak-kanakan?", "id": "Pikiran Bocah." },
  { "en": "Sangat Terbuka Wawasannya?", "id": "Wawasan Luas." },
  { "en": "Sangat Sempit Wawasannya?", "id": "Wawasan Sempit." },
  { "en": "Sangat Toleran Terhadap Perbedaan?", "id": "Hati Lapang." },
  { "en": "Sangat Tidak Toleran?", "id": "Hati Sempit." },
  { "en": "Sangat Mudah Menerima Hal Baru?", "id": "Pikiran Terbuka." },
  { "en": "Sangat Sulit Menerima Perubahan?", "id": "Pikiran Tertutup." },
  { "en": "Selalu Belajar Hal Baru?", "id": "Haus Ilmu." },
  { "en": "Merasa Sudah Paling Tahu?", "id": "Sok Tahu." },
  { "en": "Selalu Ingin Belajar?", "id": "Rendah Hati." },
  { "en": "Malas Untuk Belajar?", "id": "Otak Buntu." },
  { "en": "Sangat Cepat Belajar?", "id": "Cepat Tanggap." },
  { "en": "Sangat Lambat Belajar?", "id": "Lambat Tangkap." },
  { "en": "Punya Ingatan Kuat?", "id": "Ingatan Gajah." },
  { "en": "Sangat Pelupa?", "id": "Ingatan Ikan." },
  { "en": "Orang Yang Sangat Beruntung?", "id": "Bintang Terang." },
  { "en": "Orang Yang Selalu Sial?", "id": "Bawa Sial." },
  { "en": "Tempat Yang Sangat Aman?", "id": "Surga Dunia." },
  { "en": "Tempat Yang Sangat Berbahaya?", "id": "Sarang Penyamun." },
  { "en": "Masa-masa Sulit?", "id": "Zaman Edan." },
  { "en": "Masa-masa Indah?", "id": "Kenangan Manis." },
  { "en": "Kenangan Buruk?", "id": "Mimpi Buruk." },
  { "en": "Sesuatu Yang Sangat Dirindukan?", "id": "Obat Rindu." },
  { "en": "Sesuatu Yang Sangat Dibenci?", "id": "Musuh Dalam Selimut." },
  { "en": "Sangat Menyakitkan?", "id": "Seperti Ditusuk." },
  { "en": "Sangat Menyenangkan?", "id": "Seperti Di Surga." },
  { "en": "Sangat Melelahkan?", "id": "Remuk Redam." },
  { "en": "Sangat Menyegarkan?", "id": "Embun Pagi." },
  { "en": "Sangat Membosankan?", "id": "Mati Kutu." },
  { "en": "Sangat Menarik?", "id": "Curi Perhatian." },
  { "en": "Sangat Menjijikkan?", "id": "Bikin Mual." },
  { "en": "Sangat Menggugah Selera?", "id": "Bikin Lapar." },
  { "en": "Sangat Tidak Masuk Akal?", "id": "Di Luar Nalar." },
  { "en": "Sangat Mudah Dipercaya?", "id": "Masuk Akal." },
  { "en": "Sangat Sulit Dipercaya?", "id": "Sulit Dipercaya." },
  { "en": "Kabar Bohong Yang Meyakinkan?", "id": "Fitnah Keji." },
  { "en": "Kebenaran Yang Menyakitkan?", "id": "Pil Pahit." },
  { "en": "Kebohongan Yang Menenangkan?", "id": "Gula-gula." },
  { "en": "Pujian Yang Menjatuhkan?", "id": "Racun Manis." },
  { "en": "Kritik Yang Membangun?", "id": "Obat Pahit." },
  { "en": "Teman Yang Menusuk Dari Belakang?", "id": "Pagar Makan Tanaman." },
  { "en": "Musuh Yang Menjadi Teman?", "id": "Musuh Jadi Kawan." },
  { "en": "Orang Yang Punya Banyak Muka?", "id": "Seribu Wajah." },
  { "en": "Orang Yang Sangat Polos?", "id": "Lurus Hati." },
  { "en": "Orang Yang Sangat Licik?", "id": "Otak Licik." },
  { "en": "Sangat Pandai Berpura-pura?", "id": "Ahli Sandiwara." },
  { "en": "Sangat Jujur Dan Terbuka?", "id": "Buku Terbuka." },
  { "en": "Sangat Misterius?", "id": "Kotak Pandora." },
  { "en": "Sumber Masalah?", "id": "Akar Masalah." },
  { "en": "Penyelesaian Masalah?", "id": "Kunci Jawaban." },
  { "en": "Sesuatu Yang Sangat Berharga?", "id": "Harta Karun." },
  { "en": "Sesuatu Yang Tidak Berguna?", "id": "Barang Rongsokan." },
  { "en": "Orang Yang Sangat Dihormati?", "id": "Tokoh Panutan." },
  { "en": "Orang Yang Sangat Diremehkan?", "id": "Anak Bawang." },
  { "en": "Orang Yang Sangat Berpengaruh?", "id": "Punya Pengaruh." },
  { "en": "Orang Yang Tidak Punya Pengaruh?", "id": "Bukan Siapa-siapa." },
  { "en": "Orang Yang Punya Kekuasaan?", "id": "Pegang Kuasa." },
  { "en": "Orang Biasa Tanpa Kekuasaan?", "id": "Rakyat Biasa." },
  { "en": "Pemimpin Yang Zalim?", "id": "Tangan Besi." },
  { "en": "Pemimpin Yang Bijaksana?", "id": "Hati Emas." },
  { "en": "Pemerintahan Yang Korup?", "id": "Sarang Koruptor." },
  { "en": "Pemerintahan Yang Bersih?", "id": "Pemerintahan Bersih." },
  { "en": "Negara Yang Kaya Raya?", "id": "Negeri Subur." },
  { "en": "Negara Yang Sangat Miskin?", "id": "Negeri Tandus." },
  { "en": "Masyarakat Yang Sejahtera?", "id": "Masyarakat Adil." },
  { "en": "Masyarakat Yang Menderita?", "id": "Masyarakat Sengsara." },
  { "en": "Hukum Yang Sangat Tajam?", "id": "Pisau Keadilan." },
  { "en": "Hukum Yang Bisa Dibeli?", "id": "Hukum Dagang." },
  { "en": "Keadilan Yang Sulit Didapat?", "id": "Keadilan Langka." },
  { "en": "Ketidakadilan Yang Merajalela?", "id": "Zalim Merajalela." },
  { "en": "Perjuangan Tanpa Henti?", "id": "Api Tak Kunjung Padam." },
  { "en": "Pengorbanan Yang Sia-sia?", "id": "Garam Jatuh Ke Air." },
  { "en": "Kemenangan Yang Diraih Susah Payah?", "id": "Kemenangan Pahit." },
  { "en": "Kekalahan Yang Menyakitkan?", "id": "Kekalahan Pahit." },
  { "en": "Harapan Yang Hampir Pupus?", "id": "Benang Tipis." },
  { "en": "Keajaiban Yang Datang Tiba-tiba?", "id": "Durian Runtuh." },
  { "en": "Titik Terendah Dalam Hidup?", "id": "Dasar Jurang." },
  { "en": "Puncak Kebahagiaan?", "id": "Puncak Dunia." },
  { "en": "Perjalanan Hidup Yang Berliku?", "id": "Jalan Berduri." },
  { "en": "Masa Depan Yang Suram?", "id": "Awan Hitam." },
  { "en": "Masa Depan Yang Penuh Harapan?", "id": "Fajar Menyingsing." },
  { "en": "Awal Dari Kehancuran?", "id": "Retak Rambut." },
  { "en": "Sangat Sulit Untuk Diperbaiki?", "id": "Nasi Jadi Bubur." },
  { "en": "Kesempatan Yang Hilang?", "id": "Kereta Lewat." },
  { "en": "Mencoba Peruntungan?", "id": "Adu Nasib." },
  { "en": "Menyerah Pada Takdir?", "id": "Terima Takdir." },
  { "en": "Membuat Sejarah Baru?", "id": "Ukir Sejarah." },
  { "en": "Menjadi Legenda Hidup?", "id": "Legenda Hidup." },
  { "en": "Dikenang Selamanya?", "id": "Abadi Selamanya." },
  { "en": "Hilang Ditelan Waktu?", "id": "Lupa Waktu." },
  { "en": "Sesuatu Yang Abadi?", "id": "Batu Karang." },
  { "en": "Sesuatu Yang Fana?", "id": "Buih Di Lautan." },
  { "en": "Kehidupan Setelah Mati?", "id": "Alam Baka." },
  { "en": "Dunia Yang Penuh Tipu Daya?", "id": "Panggung Sandiwara." },
  { "en": "Orang Yang Selalu Gembira?", "id": "Muka Ceria." },
  { "en": "Orang Yang Selalu Bersedih?", "id": "Muka Muram." },
  { "en": "Sangat Sulit Untuk Dilawan?", "id": "Arus Deras." },
  { "en": "Sangat Mudah Untuk Diikuti?", "id": "Ikut Arus." },
  { "en": "Menjadi Diri Sendiri?", "id": "Jadi Diri." },
  { "en": "Menjadi Orang Lain?", "id": "Pakai Topeng." },
  { "en": "Menyembunyikan Perasaan Sebenarnya?", "id": "Simpan Rasa." },
  { "en": "Mengungkapkan Isi Hati?", "id": "Curahan Hati." },
  { "en": "Pembicaraan Dari Hati Ke Hati?", "id": "Empat Mata." },
  { "en": "Saling Memahami Tanpa Kata?", "id": "Bahasa Kalbu." },
  { "en": "Sangat Sulit Untuk Berkomunikasi?", "id": "Beda Frekuensi." },
  { "en": "Sangat Mudah Untuk Berkomunikasi?", "id": "Satu Frekuensi." },
  { "en": "Saling Mendukung Satu Sama Lain?", "id": "Bahu Membahu." },
  { "en": "Saling Menjatuhkan Satu Sama Lain?", "id": "Saling Sikut." },
  { "en": "Bekerja Sama Untuk Tujuan Bersama?", "id": "Satu Tim." },
  { "en": "Sangat Rapi Dan Teratur?", "id": "Seperti Semut." },
  { "en": "Sangat Kacau Dan Berantakan?", "id": "Kapal Pecah." },
  { "en": "Suasana Sangat Hening?", "id": "Malam Sunyi." },
  { "en": "Suasana Sangat Bising?", "id": "Pasar Kaget." },
  { "en": "Sesuatu Yang Sangat Lazim?", "id": "Sudah Biasa." },
  { "en": "Sesuatu Yang Sangat Aneh?", "id": "Luar Biasa." },
  { "en": "Kejadian Sangat Langka?", "id": "Bulan Purnama." },
  { "en": "Kejadian Sehari-hari?", "id": "Roti Harian." },
  { "en": "Orang Yang Selalu Ingin Menang Sendiri?", "id": "Egois Sekali." },
  { "en": "Orang Yang Selalu Mengalah?", "id": "Hati Lembut." },
  { "en": "Sangat Sulit Mengambil Keputusan?", "id": "Makan Buah Simalakama." },
  { "en": "Sangat Mudah Dipengaruhi?", "id": "Tiup Angin." },
  { "en": "Sangat Teguh Pendiriannya?", "id": "Batu Karang." },
  { "en": "Sangat Fleksibel?", "id": "Seperti Air." },
  { "en": "Sangat Kaku Dan Tidak Mau Berubah?", "id": "Seperti Robot." },
  { "en": "Sangat Spontan Dan Apa Adanya?", "id": "Tanpa Topeng." },
  { "en": "Selalu Berpura-pura?", "id": "Pakai Topeng." },
  { "en": "Menyimpan Rahasia Sangat Rapat?", "id": "Tutup Rapat." },
  { "en": "Sangat Suka Membicarakan Orang?", "id": "Bibir Tipis." },
  { "en": "Tidak Suka Bergosip?", "id": "Jaga Mulut." },
  { "en": "Perkataan Yang Sangat Menyakitkan?", "id": "Lidah Tajam." },
  { "en": "Perkataan Yang Sangat Menenangkan?", "id": "Sejuk Di Hati." },
  { "en": "Nasihat Yang Sangat Berguna?", "id": "Emas Murni." },
  { "en": "Nasihat Yang Tidak Berguna?", "id": "Angin Lalu." },
  { "en": "Pengalaman Adalah Guru Terbaik?", "id": "Guru Kehidupan." },
  { "en": "Tidak Belajar Dari Kesalahan?", "id": "Keledai Jatuh." },
  { "en": "Selalu Mengulangi Kesalahan?", "id": "Lubang Yang Sama." },
  { "en": "Mencari Ilmu Sampai Tua?", "id": "Belajar Seumur Hidup." },
  { "en": "Ilmu Yang Bermanfaat?", "id": "Ilmu Padi." },
  { "en": "Semakin Berisi Semakin Merunduk?", "id": "Ilmu Padi." },
  { "en": "Sombong Karena Ilmu?", "id": "Tinggi Ilmu." },
  { "en": "Cinta Buta?", "id": "Cinta Mati." },
  { "en": "Benci Tapi Rindu?", "id": "Benci Tapi Rindu." },
  { "en": "Hubungan Yang Rumit?", "id": "Benang Kusut." },
  { "en": "Hubungan Yang Harmonis?", "id": "Keluarga Cemara." },
  { "en": "Pertengkaran Kecil Dalam Rumah Tangga?", "id": "Riak Kecil." },
  { "en": "Masalah Besar Dalam Pernikahan?", "id": "Badai Perkawinan." },
  { "en": "Pasangan Yang Sangat Serasi?", "id": "Romeo Juliet." },
  { "en": "Pasangan Yang Selalu Bertengkar?", "id": "Tom And Jerry." },
  { "en": "Cinta Yang Tak Terbalas?", "id": "Tepuk Sebelah Tangan." },
  { "en": "Cinta Yang Terlarang?", "id": "Cinta Terlarang." },
  { "en": "Menikah Karena Harta?", "id": "Kawin Kontrak." },
  { "en": "Menikah Karena Cinta?", "id": "Kawin Emas." },
  { "en": "Mencari Pasangan Hidup?", "id": "Cari Jodoh." },
  { "en": "Menemukan Belahan Jiwa?", "id": "Temu Jodoh." },
  { "en": "Hidup Sendiri Sampai Tua?", "id": "Bujang Lapuk." },
  { "en": "Menjadi Janda Atau Duda?", "id": "Sendiri Lagi." },
  { "en": "Sangat Merindukan Masa Lalu?", "id": "Nostalgia." },
  { "en": "Sangat Mengkhawatirkan Masa Depan?", "id": "Cemas Berlebihan." },
  { "en": "Menikmati Saat Ini?", "id": "Hidup Hari Ini." },
  { "en": "Selalu Berpikir Jangka Panjang?", "id": "Visi Jauh." },
  { "en": "Hanya Berpikir Jangka Pendek?", "id": "Visi Pendek." },
  { "en": "Membuang Waktu Percuma?", "id": "Bakar Uang." },
  { "en": "Waktu Adalah Uang?", "id": "Waktu Adalah Uang." },
  { "en": "Memanfaatkan Setiap Detik?", "id": "Kejar Waktu." },
  { "en": "Selalu Terlambat?", "id": "Jam Karet." },
  { "en": "Datang Terlalu Awal?", "id": "Kepagian." },
  { "en": "Momen Yang Tepat?", "id": "Waktu Tepat." },
  { "en": "Salah Waktu?", "id": "Salah Timing." },
  { "en": "Keberuntungan Datang Di Waktu Tepat?", "id": "Hoki Tepat." },
  { "en": "Sial Karena Waktu Yang Salah?", "id": "Sial Timing." },
  { "en": "Sangat Detail Dan Sempurna?", "id": "Perfeksionis." },
  { "en": "Mengerjakan Sesuatu Asal Jadi?", "id": "Asal Jadi." },
  { "en": "Menjaga Kualitas Di Atas Segalanya?", "id": "Jaga Mutu." },
  { "en": "Mengejar Target Kuantitas?", "id": "Kejar Setoran." },
  { "en": "Keseimbangan Antara Kualitas Dan Kuantitas?", "id": "Jalan Tengah." },
  { "en": "Terlalu Banyak Teori?", "id": "Banyak Teori." },
  { "en": "Kurang Praktik?", "id": "Kurang Praktek." },
  { "en": "Langsung Terjun Ke Lapangan?", "id": "Turun Lapangan." },
  { "en": "Belajar Sambil Bekerja?", "id": "Magang Kerja." },
  { "en": "Mengajarkan Ikan Berenang?", "id": "Garam Ke Laut." },
  { "en": "Sesuatu Yang Sudah Jelas?", "id": "Terang Benderang." },
  { "en": "Sesuatu Yang Sangat Samar?", "id": "Abu-abu." },
  { "en": "Antara Benar Dan Salah?", "id": "Wilayah Abu-abu." },
  { "en": "Pilihan Hitam Atau Putih?", "id": "Hitam Putih." },
  { "en": "Tidak Ada Pilihan Lain?", "id": "Satu Jalan." },
  { "en": "Berada Dalam Posisi Sulit?", "id": "Posisi Sulit." },
  { "en": "Memiliki Posisi Tawar Kuat?", "id": "Kartu As." },
  { "en": "Tidak Punya Kekuatan Tawar?", "id": "Kartu Mati." },
  { "en": "Memainkan Kartu Dengan Baik?", "id": "Main Cantik." },
  { "en": "Salah Langkah?", "id": "Salah Langkah." },
  { "en": "Mengambil Langkah Berani?", "id": "Langkah Kuda." },
  { "en": "Langkah Mundur?", "id": "Langkah Mundur." },
  { "en": "Lompatan Besar Ke Depan?", "id": "Lompatan Jauh." },
  { "en": "Kemajuan Yang Sangat Lambat?", "id": "Langkah Siput." },
  { "en": "Perubahan Drastis?", "id": "Revolusi." },
  { "en": "Perubahan Bertahap?", "id": "Evolusi." },
  { "en": "Kembali Ke Titik Awal?", "id": "Kembali Ke Nol." },
  { "en": "Mencapai Tujuan Akhir?", "id": "Garis Finis." },
  { "en": "Perjalanan Masih Panjang?", "id": "Jalan Panjang." },
  { "en": "Menikmati Setiap Prosesnya?", "id": "Nikmati Perjalanan." },
  { "en": "Hanya Fokus Pada Hasil?", "id": "Fokus Hasil." },
  { "en": "Menghalalkan Segala Cara?", "id": "Jalan Pintas." },
  { "en": "Mengikuti Jalan Yang Benar?", "id": "Jalan Lurus." },
  { "en": "Tersesat Di Jalan Yang Salah?", "id": "Salah Jalan." },
  { "en": "Mendapat Hidayah?", "id": "Dapat Cahaya." },
  { "en": "Orang Yang Selalu Berpindah Tempat Tinggal?", "id": "Rumah Berjalan." },
  { "en": "Sangat Betah Di Suatu Tempat?", "id": "Sudah Berakar." },
  { "en": "Sangat Rindu Kampung Halaman?", "id": "Anak Rantau." },
  { "en": "Suka Berpetualang?", "id": "Jiwa Petualang." },
  { "en": "Lebih Suka Tinggal Di Rumah?", "id": "Anak Rumahan." },
  { "en": "Sangat Luas Pergaulannya?", "id": "Kupu-kupu Malam." },
  { "en": "Sangat Terbatas Pergaulannya?", "id": "Dalam Tempurung." },
  { "en": "Selalu Mengikuti Tren Terbaru?", "id": "Kekinian." },
  { "en": "Sangat Tertinggal Informasi?", "id": "Kudet Sekali." },
  { "en": "Menjadi Viral Di Media Sosial?", "id": "Jadi Viral." },
  { "en": "Menjadi Bahan Cemoohan?", "id": "Bulan-bulanan." },
  { "en": "Mendapat Banyak Pujian?", "id": "Banjir Pujian." },
  { "en": "Mendapat Banyak Kritikan?", "id": "Hujan Kritik." },
  { "en": "Tetap Kuat Meski Dikritik?", "id": "Telinga Baja." },
  { "en": "Sangat Mudah Tersinggung?", "id": "Hati Kaca." },
  { "en": "Sangat Berhati-hati Dalam Bertindak?", "id": "Langkah Semut." },
  { "en": "Sangat Ceroboh Dan Gegabah?", "id": "Tangan Gatal." },
  { "en": "Selalu Memikirkan Risiko?", "id": "Penuh Perhitungan." },
  { "en": "Berani Menanggung Akibat?", "id": "Siap Terima." },
  { "en": "Selalu Mencari Aman?", "id": "Zona Aman." },
  { "en": "Menyukai Tantangan?", "id": "Suka Tantangan." },
  { "en": "Mengambil Jalan Yang Sulit?", "id": "Jalan Terjal." },
  { "en": "Mencari Kemudahan?", "id": "Jalan Mulus." },
  { "en": "Tidak Ada Usaha Tanpa Hasil?", "id": "Ada Usaha." },
  { "en": "Keberhasilan Tidak Datang Sendiri?", "id": "Tidak Instan." },
  { "en": "Perlu Proses Dan Perjuangan?", "id": "Butuh Proses." },
  { "en": "Sangat Menghargai Proses?", "id": "Hargai Proses." },
  { "en": "Hanya Mau Hasil Instan?", "id": "Mau Instan." },
  { "en": "Kesuksesan Semalam?", "id": "Sukses Semalam." },
  { "en": "Kerja Keras Bertahun-tahun?", "id": "Kerja Keras." },
  { "en": "Membangun Reputasi Baik?", "id": "Bangun Nama." },
  { "en": "Merusak Reputasi Dalam Sekejap?", "id": "Rusak Nama." },
  { "en": "Kepercayaan Sulit Didapat?", "id": "Kepercayaan Mahal." },
  { "en": "Sangat Mudah Kehilangan Kepercayaan?", "id": "Hilang Percaya." },
  { "en": "Menjaga Amanah?", "id": "Jaga Amanah." },
  { "en": "Mengkhianati Amanah?", "id": "Khianat Amanah." },
  { "en": "Sangat Bisa Diandalkan?", "id": "Bisa Diandalkan." },
  { "en": "Sangat Tidak Bisa Diandalkan?", "id": "Angin-anginan." },
  { "en": "Selalu Ada Saat Dibutuhkan?", "id": "Teman Sejati." },
  { "en": "Menghilang Saat Dibutuhkan?", "id": "Teman Palsu." },
  { "en": "Datang Saat Ada Perlu?", "id": "Ada Maunya." },
  { "en": "Tulus Tanpa Syarat?", "id": "Tanpa Pamrih." },
  { "en": "Selalu Mengharap Imbalan?", "id": "Hitung-hitungan." },
  { "en": "Kebaikan Yang Dilupakan?", "id": "Susu Dibalas Tuba." },
  { "en": "Kejahatan Yang Tak Terbalas?", "id": "Hukum Karma." },
  { "en": "Keadilan Akan Menang?", "id": "Kebenaran Menang." },
  { "en": "Kejahatan Akan Terungkap?", "id": "Sepandai Tupai." },
  { "en": "Menyembunyikan Bangkai?", "id": "Bau Busuk." },
  { "en": "Tidak Ada Rahasia Yang Abadi?", "id": "Rahasia Terbongkar." },
  { "en": "Waktu Akan Menjawab?", "id": "Waktu Menjawab." },
  { "en": "Biarkan Alam Yang Menyeleksi?", "id": "Seleksi Alam." },
  { "en": "Yang Kuat Yang Bertahan?", "id": "Hukum Rimba." },
  { "en": "Yang Cerdik Yang Menang?", "id": "Akal Bulus." },
  { "en": "Kekuatan Bukan Segalanya?", "id": "Otak Di Atas." },
  { "en": "Kecerdasan Mengalahkan Kekuatan?", "id": "Akal Kancil." },
  { "en": "Menggunakan Otot Bukan Otak?", "id": "Kepala Kosong." },
  { "en": "Menggunakan Otak Dan Hati?", "id": "Cerdas Hati." },
  { "en": "Keseimbangan Antara Logika Dan Perasaan?", "id": "Jalan Tengah." },
  { "en": "Terlalu Mengikuti Perasaan?", "id": "Terbawa Perasaan." },
  { "en": "Terlalu Kaku Dengan Logika?", "id": "Seperti Mesin." },
  { "en": "Manusiawi Dan Penuh Empati?", "id": "Punya Hati." },
  { "en": "Tidak Punya Hati Nurani?", "id":"Hati Mati." },
  { "en": "Berbuat Baik Tanpa Dilihat?", "id": "Tangan Kanan Tangan Kiri." },
  { "en": "Berbuat Baik Untuk Pamer?", "id": "Riya." },
  { "en": "Ibadah Yang Khusyuk?", "id": "Ibadah Tenang." },
  { "en": "Ibadah Hanya Formalitas?", "id": "Ibadah Palsu." },
  { "en": "Keimanan Yang Kuat?", "id": "Iman Baja." },
  { "en": "Keimanan Yang Rapuh?", "id": "Iman Tipis." },
  { "en": "Digoda Oleh Setan?", "id": "Godaan Setan." },
  { "en": "Melawan Hawa Nafsu?", "id": "Lawan Nafsu." },
  { "en": "Tergoda Duniawi?", "id": "Mabuk Dunia." },
  { "en": "Mengingat Kematian?", "id": "Ingat Mati." },
  { "en": "Mempersiapkan Bekal Akhirat?", "id": "Bekal Akhirat." },
  { "en": "Hidup Hanya Sementara?", "id": "Dunia Fana." },
  { "en": "Akhirat Selama-lamanya?", "id": "Akhirat Abadi." },
  { "en": "Mencari Ridha Tuhan?", "id": "Cari Ridha." },
  { "en": "Takut Akan Azab Tuhan?", "id": "Takut Azab." },
  { "en": "Berharap Rahmat Tuhan?", "id": "Harap Rahmat." },
  { "en": "Berserah Diri Pada Tuhan?", "id": "Tawakal." },
  { "en": "Berusaha Sekuat Tenaga?", "id": "Ikhtiar." },
  { "en": "Doa Tanpa Usaha?", "id": "Doa Kosong." },
  { "en": "Usaha Tanpa Doa?", "id": "Usaha Sombong." },
  { "en": "Kombinasi Usaha Dan Doa?", "id": "Kunci Sukses." },
  { "en": "Selalu Bersyukur?", "id": "Hati Bersyukur." },
  { "en": "Selalu Merasa Cukup?", "id": "Qanaah." },
  { "en": "Selalu Iri Hati?", "id": "Hati Dengki." },
  { "en": "Senang Melihat Orang Susah?", "id": "Senang Di Atas." },
  { "en": "Susah Melihat Orang Senang?", "id": "Susah Lihat Senang." },
  { "en": "Suka Menjatuhkan Orang Lain?", "id": "Suka Menjatuhkan." },
  { "en": "Suka Membantu Orang Bangkit?", "id": "Tangan Penolong." },
  { "en": "Menjadi Inspirasi Bagi Banyak Orang?", "id": "Cahaya Harapan." },
  { "en": "Menjadi Mimpi Buruk?", "id": "Momok Menakutkan." },
  { "en": "Menebar Kebaikan?", "id": "Tanam Kebaikan." },
  { "en": "Menuai Kebaikan?", "id": "Tuai Kebaikan." },
  { "en": "Menebar Angin?", "id": "Tebar Angin." },
  { "en": "Menuai Badai?", "id": "Tuai Badai." },
  { "en": "Sangat Terkejut Dan Bingung?", "id": "Mati Angin." },
  { "en": "Tidak Ada Kemajuan?", "id": "Jalan Di Tempat." },
  { "en": "Sangat Sulit Dinasihati?", "id": "Kepala Udang." },
  { "en": "Orang Yang Selalu Membangkang?", "id": "Anak Sapi." },
  { "en": "Menjadi Korban Perasaan?", "id": "Baper." },
  { "en": "Sangat Berlebihan Dalam Merespon?", "id": "Drama Queen." },
  { "en": "Selalu Ingin Menjadi Pusat Dunia?", "id": "Ego Sentris." },
  { "en": "Sangat Mementingkan Penampilan?", "id": "Jaga Imej." },
  { "en": "Asli Tanpa Rekayasa?", "id": "Apa Adanya." },
  { "en": "Penuh Kepalsuan?", "id": "Dunia Panggung." },
  { "en": "Kehidupan Yang Penuh Kemewahan?", "id": "Gemerlap Dunia." },
  { "en": "Sisi Gelap Kehidupan?", "id": "Dunia Hitam." },
  { "en": "Terjebak Dalam Lingkaran Kriminal?", "id": "Lingkaran Setan." },
  { "en": "Berhasil Keluar Dari Masalah?", "id": "Lepas Dari Jerat." },
  { "en": "Memulai Kehidupan Baru Yang Bersih?", "id": "Lembaran Baru." },
  { "en": "Masa Lalu Yang Ingin Dilupakan?", "id": "Sejarah Kelam." },
  { "en": "Menghadapi Masa Depan?", "id": "Tatapan Ke Depan." },
  { "en": "Tidak Mau Melihat Ke Belakang?", "id": "Lupakan Masa Lalu." },
  { "en": "Terjebak Di Masa Lalu?", "id": "Gagal Move On." },
  { "en": "Selalu Memikirkan Kemungkinan Terburuk?", "id": "Pikiran Negatif." },
  { "en": "Melihat Segala Sesuatu Dengan Cerah?", "id": "Kacamata Pink." },
  { "en": "Sangat Realistis?", "id": "Membumi." },
  { "en": "Suka Berandai-andai?", "id": "Tukang Mimpi." },
  { "en": "Pekerja Keras Yang Ulet?", "id": "Tulang Kawat." },
  { "en": "Sangat Mudah Menyerah?", "id": "Mental Tempe." },
  { "en": "Memiliki Mental Baja?", "id": "Mental Juara." },
  { "en": "Selalu Berpikir Sebelum Bicara?", "id": "Jaga Lidah." },
  { "en": "Berbicara Tanpa Dipikir?", "id": "Ceplas-ceplos." },
  { "en": "Perkataan Yang Tidak Bisa Ditarik?", "id": "Nasi Jadi Bubur." },
  { "en": "Menyesal Setelah Berbuat?", "id": "Sesal Kemudian." },
  { "en": "Sangat Berhati-hati?", "id": "Ada Semut." },
  { "en": "Mencari Kesalahan Kecil?", "id": "Semut Di Seberang." },
  { "en": "Tidak Melihat Kesalahan Sendiri?", "id": "Gajah Di Pelupuk." },
  { "en": "Suka Mengoreksi Orang Lain?", "id": "Tukang Kritik." },
  { "en": "Tidak Mau Dikritik?", "id": "Anti Kritik." },
  { "en": "Menerima Masukan Dengan Baik?", "id": "Terbuka Untuk Masukan." },
  { "en": "Sangat Keras Kepala?", "id": "Tutup Telinga." },
  { "en": "Sangat Mudah Belajar?", "id": "Cepat Tangkap." },
  { "en": "Sangat Sulit Mengerti?", "id": "Lambat Panas." },
  { "en": "Memiliki Banyak Bakat?", "id": "Multi Talenta." },
  { "en": "Fokus Pada Satu Keahlian?", "id": "Spesialis." },
  { "en": "Mengetahui Sedikit Tentang Banyak Hal?", "id": "Generalis." },
  { "en": "Sangat Ahli Tapi Tidak Dikenal?", "id": "Mutiara Terpendam." },
  { "en": "Orang Terkenal Tanpa Keahlian?", "id": "Artis Instan." },
  { "en": "Kesuksesan Yang Tidak Bertahan Lama?", "id": "Hangat-hangat Tahi Ayam." },
  { "en": "Karya Yang Abadi?", "id": "Mahakarya." },
  { "en": "Meniru Karya Orang Lain?", "id": "Jiwa Plagiat." },
  { "en": "Memiliki Ide Orisinal?", "id": "Ide Segar." },
  { "en": "Selalu Punya Ide Baru?", "id": "Pabrik Ide." },
  { "en": "Kehabisan Akal?", "id": "Buntu Akal." },
  { "en": "Mencari Inspirasi?", "id": "Cari Ilham." },
  { "en": "Bekerja Di Bawah Tekanan?", "id": "Sistem Kebut." },
  { "en": "Bekerja Paling Baik Saat Santai?", "id": "Kerja Santai." },
  { "en": "Hasil Maksimal Dengan Usaha Minimal?", "id": "Kerja Cerdas." },
  { "en": "Usaha Maksimal Hasil Minimal?", "id": "Kerja Keras." },
  { "en": "Kombinasi Kerja Keras Dan Cerdas?", "id": "Kerja Tuntas." },
  { "en": "Menikmati Buah Kerja Keras?", "id": "Panen Raya." },
  { "en": "Gagal Total?", "id": "Gagal Panen." },
  { "en": "Mengambil Risiko Besar?", "id": "Bertaruh Besar." },
  { "en": "Menghindari Segala Risiko?", "id": "Main Aman." },
  { "en": "Untung Besar?", "id": "Jackpot." },
  { "en": "Rugi Besar?", "id": "Rugi Bandar." },
  { "en": "Kembali Modal?", "id": "Balik Modal." },
  { "en": "Tidak Untung Tidak Rugi?", "id": "Impas." },
  { "en": "Bisnis Yang Menjanjikan?", "id": "Lahan Basah." },
  { "en": "Bisnis Yang Sulit?", "id": "Lahan Kering." },
  { "en": "Persaingan Yang Sangat Ketat?", "id": "Saling Sikut." },
  { "en": "Persaingan Sehat?", "id": "Kompetisi Sehat." },
  { "en": "Menjatuhkan Pesaing?", "id": "Main Kotor." },
  { "en": "Menghormati Pesaing?", "id": "Sikap Ksatria." },
  { "en": "Mengakui Kekalahan?", "id": "Berjiwa Besar." },
  { "en": "Tidak Terima Kalah?", "id": "Tidak Sportif." },
  { "en": "Mencari-cari Alasan Kekalahan?", "id": "Banyak Alasan." },
  { "en": "Menerima Kemenangan Dengan Rendah Hati?", "id": "Tetap Membumi." },
  { "en": "Sombong Saat Menang?", "id": "Lupa Diri." },
  { "en": "Menjadi Teladan Bagi Yang Lain?", "id": "Jadi Contoh." },
  { "en": "Menjadi Aib Bagi Kelompok?", "id": "Nila Setitik." },
  { "en": "Merusak Reputasi Seluruh Kelompok?", "id": "Rusak Susu." },
  { "en": "Menjaga Nama Baik Kelompok?", "id": "Jaga Nama Baik." },
  { "en": "Solidaritas Yang Sangat Kuat?", "id": "Satu Rasa." },
  { "en": "Mementingkan Diri Sendiri?", "id": "Tidak Solidar." },
  { "en": "Berjuang Untuk Kepentingan Bersama?", "id": "Kepentingan Umum." },
  { "en": "Mengorbankan Kepentingan Pribadi?", "id": "Berjiwa Sosial." },
  { "en": "Menjadi Relawan Kemanusiaan?", "id": "Tangan Tuhan." },
  { "en": "Orang Yang Sangat Dermawan?", "id": "Murah Hati." },
  { "en": "Sangat Pelit?", "id": "Tangan Keras." },
  { "en": "Selalu Membantu Orang Dalam Kesulitan?", "id": "Pahlawan Kesiangan." },
  { "en": "Tidak Peduli Penderitaan Orang?", "id": "Hati Beku." },
  { "en": "Sangat Mudah Merasa Iba?", "id": "Hati Tipis." },
  { "en": "Sangat Tega Dan Kejam?", "id": "Hati Iblis." },
  { "en": "Berwajah Malaikat Berhati Iblis?", "id": "Serigala Berbulu." },
  { "en": "Terlihat Baik Tapi Jahat?", "id": "Musang Berbulu Ayam." },
  { "en": "Terlihat Buruk Tapi Baik?", "id": "Durian Runtuh." },
  { "en": "Jangan Menilai Dari Luar?", "id": "Jangan Lihat Sampul." },
  { "en": "Orang Yang Pandai Bicara?", "id": "Pintar Lidah." },
  { "en": "Tidak Bisa Berkata-kata?", "id": "Kelu Lidah." },
  { "en": "Perkataan Yang Tidak Berarti?", "id": "Omong Kosong." },
  { "en": "Janji Yang Diucapkan Saja?", "id": "Janji Manis." },
  { "en": "Sumpah Palsu?", "id": "Sumpah Serapah." },
  { "en": "Berpegang Teguh Pada Sumpah?", "id": "Sumpah Mati." },
  { "en": "Berani Mati Demi Prinsip?", "id": "Harga Mati." },
  { "en": "Sangat Mudah Mengubah Prinsip?", "id": "Tiada Pendirian." },
  { "en": "Selalu Berubah Pikiran?", "id": "Pikiran Goyah." },
  { "en": "Sangat Konsisten?", "id": "Satu Arah." },
  { "en": "Fokus Dan Tidak Tergoyahkan?", "id": "Fokus Tujuan." },
  { "en": "Mudah Tergoda?", "id": "Iman Lemah." },
  { "en": "Kuat Menahan Godaan?", "id": "Iman Kuat." },
  { "en": "Menguji Keimanan Seseorang?", "id": "Ujian Hidup." },
  { "en": "Lulus Dari Ujian?", "id": "Naik Kelas." },
  { "en": "Tidak Lulus Ujian?", "id": "Tinggal Kelas." },
  { "en": "Mengulang Pelajaran?", "id": "Belajar Lagi." },
  { "en": "Mendapat Pelajaran Berharga?", "id": "Pelajaran Hidup." },
  { "en": "Pengalaman Pahit?", "id": "Pil Pahit." },
  { "en": "Masa Depan Cerah Menanti?", "id": "Matahari Esok." },
  { "en": "Tidak Ada Harapan?", "id": "Malam Gelap." },
  { "en": "Secercah Harapan?", "id": "Titik Terang." },
  { "en": "Dalam Situasi Sangat Sulit?", "id": "Ujung Tanduk." },
  { "en": "Selamat Dari Bahaya?", "id": "Lepas Dari Maut." },
  { "en": "Mencari Maut?", "id": "Main Api." },
  { "en": "Bermain Dengan Bahaya?", "id": "Jalan Di Atas." },
  { "en": "Sangat Berisiko?", "id": "Risiko Tinggi." },
  { "en": "Sangat Aman?", "id": "Bebas Risiko." },
  { "en": "Investasi Yang Aman?", "id": "Surga Investasi." },
  { "en": "Investasi Bodong?", "id": "Angin Surga." },
  { "en": "Janji Manis Investasi?", "id": "Iming-iming." },
  { "en": "Tertipu Habis-habisan?", "id": "Ludes Semua." },
  { "en": "Belajar Dari Penipuan?", "id": "Jadi Pelajaran." },
  { "en": "Lebih Berhati-hati?", "id": "Lebih Waspada." },
  { "en": "Selalu Waspada?", "id": "Mata Batin." },
  { "en": "Punya Intuisi Tajam?", "id": "Firasat Kuat." },
  { "en": "Mengikuti Kata Hati?", "id": "Suara Hati." },
  { "en": "Mengabaikan Firasat?", "id": "Abaikan Firasat." },
  { "en": "Terbukti Benar?", "id": "Ternyata Benar." },
  { "en": "Perkiraan Yang Meleset?", "id": "Salah Prediksi." },
  { "en": "Ahli Membuat Prediksi?", "id": "Tangan Dewa." },
  { "en": "Selalu Tepat Sasaran?", "id": "Tembak Jitu." },
  { "en": "Selalu Gagal Mencapai Target?", "id": "Gagal Total." },
  { "en": "Memasang Target Terlalu Tinggi?", "id": "Mimpi Ketinggian." },
  { "en": "Target Yang Realistis?", "id": "Target Realistis." },
  { "en": "Langkah Demi Langkah?", "id": "Satu Per Satu." },
  { "en": "Ingin Cepat Sukses?", "id": "Jalan Instan." },
  { "en": "Tidak Ada Jalan Pintas?", "id": "Tiada Jalan." },
  { "en": "Semua Butuh Pengorbanan?", "id": "Ada Harga." },
  { "en": "Pengorbanan Yang Sebanding?", "id": "Harga Pantas." },
  { "en": "Pengorbanan Yang Terlalu Besar?", "id": "Harga Mahal." },
  { "en": "Mendapat Sesuatu Tanpa Usaha?", "id": "Dapat Gratis." },
  { "en": "Tidak Ada Makan Siang Gratis?", "id": "Tiada Gratis." },
  { "en": "Selalu Ada Udang Di Balik Batu?", "id": "Ada Udang." },
  { "en": "Niat Tersembunyi?", "id": "Maksud Terselubung." },
  { "en": "Tujuan Yang Sebenarnya?", "id": "Tujuan Asli." },
  { "en": "Berpura-pura Tidak Tahu?", "id": "Muka Polos." },
  { "en": "Sangat Licik Dan Penuh Rencana?", "id": "Seribu Akal." },
  { "en": "Selalu Punya Jalan Keluar?", "id": "Banyak Jalan." },
  { "en": "Terjebak Dan Tidak Bisa Keluar?", "id": "Masuk Perangkap." },
  { "en": "Membuat Perangkap Untuk Musuh?", "id": "Pasang Jebakan." },
  { "en": "Terkena Jebakan Sendiri?", "id": "Senjata Makan Tuan." },
  { "en": "Niat Buruk Berbalik?", "id": "Karma Instan." },
  { "en": "Perbuatan Baik Dibalas Baik?", "id": "Tanam Tuai." },
  { "en": "Kebaikan Yang Tulus?", "id": "Hati Ikhlas." },
  { "en": "Kejahatan Yang Direncanakan?", "id": "Rencana Jahat." },
  { "en": "Bertindak Spontan?", "id": "Tanpa Pikir." },
  { "en": "Mengikuti Arus?", "id": "Ikut Saja." },
  { "en": "Melawan Arus?", "id": "Beda Sendiri." },
  { "en": "Menjadi Pemimpin Opini?", "id": "Trend Setter." },
  { "en": "Menjadi Pengikut?", "id": "Follower." },
  { "en": "Punya Pengaruh Besar?", "id": "Punya Nama." },
  { "en": "Tidak Dianggap?", "id": "Angin Lewat." },
  { "en": "Sangat Dihargai?", "id": "Dihormati." },
  { "en": "Tidak Dihargai Sama Sekali?", "id": "Tidak Dilihat." },
  { "en": "Berjuang Untuk Mendapat Pengakuan?", "id": "Cari Pengakuan." },
  { "en": "Bekerja Dalam Diam?", "id": "Kerja Sunyi." },
  { "en": "Hasil Karya Yang Berbicara?", "id": "Karya Bicara." },
  { "en": "Banyak Bicara Tanpa Hasil?", "id": "Omdo." },
  { "en": "Membuktikan Dengan Tindakan?", "id": "Buktikan Saja." },
  { "en": "Janji Adalah Hutang?", "id": "Janji Hutang." },
  { "en": "Sangat Mudah Berjanji?", "id": "Obral Janji." },
  { "en": "Sulit Untuk Menepati?", "id": "Sulit Tepati." },
  { "en": "Orang Yang Bisa Dipercaya?", "id": "Bisa Dipegang." },
  { "en": "Menggantungkan Harapan?", "id": "Gantung Harapan." },
  { "en": "Sumber Harapan Satu-satunya?", "id": "Satu-satunya Asa." },
  { "en": "Hancur Berkeping-keping?", "id": "Lebur Hati." },
  { "en": "Membangun Kembali Dari Awal?", "id": "Bangun Lagi." },
  { "en": "Tidak Pernah Terlambat Untuk Memulai?", "id": "Tiada Kata." },
  { "en": "Selagi Ada Kemauan?", "id": "Ada Jalan." },
  { "en": "Dimana Ada Kemauan Disitu Ada Jalan?", "id": "Ada Jalan." },
  { "en": "Kemauan Yang Keras?", "id": "Niat Baja." },
  { "en": "Mudah Patah Semangat?", "id": "Mental Kerupuk." },
  { "en": "Semangat Yang Berkobar-kobar?", "id": "Semangat Empat Lima." },
  { "en": "Kehilangan Gairah Hidup?", "id": "Mati Rasa." },
  { "en": "Menemukan Kembali Tujuan Hidup?", "id": "Temukan Arah." },
  { "en": "Orang Yang Selalu Menyendiri?", "id": "Penyendiri." },
  { "en": "Sangat Suka Keramaian?", "id": "Anak Pesta." },
  { "en": "Sangat Canggung Dalam Pergaulan?", "id": "Kaku Sekali." },
  { "en": "Sangat Pandai Mencairkan Suasana?", "id": "Pencair Suasana." },
  { "en": "Selalu Menjadi Perhatian?", "id": "Bintang Acara." },
  { "en": "Tidak Suka Menjadi Sorotan?", "id": "Belakang Layar." },
  { "en": "Bekerja Keras Di Balik Kesuksesan?", "id": "Orang Dapur." },
  { "en": "Orang Yang Tampil Di Depan?", "id": "Wajah Perusahaan." },
  { "en": "Juru Bicara Tidak Resmi?", "id": "Corong." },
  { "en": "Penyebar Informasi?", "id": "Agen Informasi." },
  { "en": "Sumber Informasi Terpercaya?", "id": "Tangan Pertama." },
  { "en": "Informasi Dari Orang Lain?", "id": "Tangan Kedua." },
  { "en": "Saksi Yang Melihat Langsung?", "id": "Saksi Mata." },
  { "en": "Memberi Keterangan Berbelit?", "id": "Berbelit-belit." },
  { "en": "Memberi Alibi Palsu?", "id": "Alibi Palsu." },
  { "en": "Terpojok Dan Tidak Bisa Mengelak?", "id": "Kena Skakmat." },
  { "en": "Menemukan Bukti Kuat?", "id": "Bukti Tak Terbantahkan." },
  { "en": "Membuka Kasus Lama?", "id": "Buka Berkas Lama." },
  { "en": "Menutupi Kejahatan?", "id": "Operasi Senyap." },
  { "en": "Konspirasi Tingkat Tinggi?", "id": "Permainan Elit." },
  { "en": "Rakyat Kecil Menjadi Korban?", "id": "Rumput Kering." },
  { "en": "Pertarungan Antar Penguasa?", "id": "Gajah Bertarung." },
  { "en": "Yang Lemah Selalu Kalah?", "id": "Hukum Alam." },
  { "en": "Keadilan Hanya Untuk Yang Kaya?", "id": "Hukum Tumpul." },
  { "en": "Hukum Tajam Ke Bawah?", "id": "Tajam Ke Bawah." },
  { "en": "Mencari Suaka Politik?", "id": "Cari Suaka." },
  { "en": "Diasingkan Dari Negara Sendiri?", "id": "Buang Negara." },
  { "en": "Kembali Ke Tanah Air?", "id": "Pulang Kandang." },
  { "en": "Sangat Nasionalis?", "id": "Merah Putih." },
  { "en": "Menjunjung Tinggi Budaya Lokal?", "id": "Cinta Produk." },
  { "en": "Terpengaruh Budaya Asing?", "id": "Kebarat-baratan." },
  { "en": "Kehilangan Identitas Budaya?", "id": "Lupa Akar." },
  { "en": "Mencari Akar Budaya?", "id": "Kembali Ke Akar." },
  { "en": "Perpaduan Dua Budaya?", "id": "Akulturasi." },
  { "en": "Budaya Baru Muncul?", "id": "Budaya Hibrida." },
  { "en": "Menghargai Keberagaman?", "id": "Bhinneka Tunggal Ika." },
  { "en": "Tidak Suka Perbedaan?", "id": "Monokultur." },
  { "en": "Memaksakan Budaya Sendiri?", "id": "Imperialisme Budaya." },
  { "en": "Menjaga Warisan Leluhur?", "id": "Jaga Warisan." },
  { "en": "Melupakan Sejarah?", "id": "Jasmerah." },
  { "en": "Belajar Dari Kesalahan Masa Lalu?", "id": "Cermin Sejarah." },
  { "en": "Mengulang Kesalahan Yang Sama?", "id": "Keledai Dungu." },
  { "en": "Generasi Penerus Bangsa?", "id": "Tunas Bangsa." },
  { "en": "Generasi Tua?", "id": "Angkatan Tua." },
  { "en": "Kesenjangan Antar Generasi?", "id": "Jurang Generasi." },
  { "en": "Menjembatani Perbedaan Generasi?", "id": "Jembatan Generasi." },
  { "en": "Yang Muda Yang Berkarya?", "id": "Darah Segar." },
  { "en": "Yang Tua Yang Bijaksana?", "id": "Makan Garam." },
  { "en": "Kolaborasi Antar Generasi?", "id": "Dua Dunia." },
  { "en": "Transfer Pengetahuan?", "id": "Estafet Ilmu." },
  { "en": "Regenerasi Kepemimpinan?", "id": "Ganti Generasi." },
  { "en": "Pemimpin Masa Depan?", "id": "Bibit Unggul." },
  { "en": "Merusak Generasi Muda?", "id": "Racun Generasi." },
  { "en": "Pendidikan Karakter Sejak Dini?", "id": "Pondasi Kuat." },
  { "en": "Keluarga Adalah Sekolah Pertama?", "id": "Sekolah Pertama." },
  { "en": "Peran Ibu Sangat Penting?", "id": "Tiang Negara." },
  { "en": "Kasih Ibu Sepanjang Masa?", "id": "Kasih Ibu." },
  { "en": "Doa Orang Tua Sangat Mustajab?", "id": "Doa Ibu." },
  { "en": "Anak Yang Membawa Berkah?", "id": "Pembawa Rezeki." },
  { "en": "Rezeki Tidak Akan Tertukar?", "id": "Sudah Diatur." },
  { "en": "Tuhan Tidak Pernah Tidur?", "id": "Maha Melihat." },
  { "en": "Selalu Ada Jalan?", "id": "Tuhan Bantu." },
  { "en": "Jangan Pernah Putus Asa?", "id": "Harapan Selalu." },
  { "en": "Setelah Kesulitan Ada Kemudahan?", "id": "Habis Gelap." },
  { "en": "Akan Ada Pelangi?", "id": "Setelah Hujan." },
  { "en": "Badai Pasti Berlalu?", "id": "Badai Berlalu." },
  { "en": "Tetap Kuat Dan Sabar?", "id": "Hati Tabah." },
  { "en": "Mengeluh Tidak Menyelesaikan?", "id": "Jangan Mengeluh." },
  { "en": "Lebih Baik Bertindak?", "id": "Aksi Nyata." },
  { "en": "Jangan Hanya Bicara?", "id": "Jangan Omong." },
  { "en": "Mulai Dari Diri Sendiri?", "id": "Mulai Dari Diri." },
  { "en": "Perubahan Kecil Berdampak Besar?", "id": "Efek Kupu-kupu." },
  { "en": "Satu Kebaikan Dibalas Seribu?", "id": "Satu Untuk Seribu." },
  { "en": "Energi Positif Menular?", "id": "Aura Positif." },
  { "en": "Lingkungan Mempengaruhi Diri?", "id": "Cerminan Lingkungan." },
  { "en": "Berteman Dengan Penjual Minyak Wangi?", "id": "Kecipratan Wangi." },
  { "en": "Salah Pilih Pergaulan?", "id": "Salah Gaul." },
  { "en": "Terjerumus Ke Hal Negatif?", "id": "Jatuh Terperosok." },
  { "en": "Masa Depan Hancur?", "id": "Masa Depan Suram." },
  { "en": "Menyesal Tiada Guna?", "id": "Sesal Tak Berguna." },
  { "en": "Waktu Tidak Bisa Diputar?", "id": "Waktu Tak Kembali." },
  { "en": "Manfaatkan Waktu Sebaik Mungkin?", "id": "Hargai Waktu." },
  { "en": "Hidup Hanya Sekali?", "id": "Hidup Sekali." },
  { "en": "Buatlah Jadi Berarti?", "id": "Jadikan Berarti." },
  { "en": "Meninggalkan Jejak Kebaikan?", "id": "Tinggalkan Jejak." },
  { "en": "Dikenang Karena Karyanya?", "id": "Dikenang Karya." },
  { "en": "Manusia Pergi Karya Tinggal?", "id": "Gajah Mati." },
  { "en": "Harimau Mati Meninggalkan Belang?", "id": "Harimau Mati." },
  { "en": "Nama Baik Lebih Berharga?", "id": "Nama Baik." },
  { "en": "Harta Bisa Dicari?", "id": "Harta Bisa Dicari." },
  { "en": "Kesehatan Nomor Satu?", "id": "Sehat Itu Mahal." },
  { "en": "Mencegah Lebih Baik Dari Mengobati?", "id": "Sedia Payung." },
  { "en": "Keluarga Adalah Harta?", "id": "Harta Paling." },
  { "en": "Teman Sejati Sulit Ditemukan?", "id": "Teman Langka." },
  { "en": "Musuh Seribu Terlalu Banyak?", "id": "Seribu Musuh." },
  { "en": "Satu Sahabat Terlalu Sedikit?", "id": "Satu Sahabat." },
  { "en": "Jaga Hubungan Baik?", "id": "Jaga Silaturahmi." },
  { "en": "Sangat Berpengalaman Hidup?", "id": "Makan Asam Garam." },
  { "en": "Mencari Masalah Tersembunyi?", "id": "Api Dalam Sekam." },
  { "en": "Memperburuk Keadaan?", "id": "Siram Bensin." },
  { "en": "Menghilang Tanpa Jejak?", "id": "Ditelan Bumi." },
  { "en": "Muncul Serentak Dimana-mana?", "id": "Musim Jamur." },
  { "en": "Menjadi Beban Pikiran?", "id": "Duri Dalam Daging." },
  { "en": "Tidak Cocok Sama Sekali?", "id": "Minyak Dan Air." },
  { "en": "Sangat Mirip Wajahnya?", "id": "Pinang Dibelah Dua." },
  { "en": "Selalu Bersama?", "id": "Surat Dan Perangko." },
  { "en": "Sangat Akrab?", "id": "Kuku Dan Daging." },
  { "en": "Berpikir Sangat Keras?", "id": "Peras Otak." },
  { "en": "Mencari Penghidupan?", "id": "Mencari Napas." },
  { "en": "Sangat Rakus Dan Serakah?", "id": "Besar Perut." },
  { "en": "Sangat Gila Harta?", "id": "Hamba Uang." },
  { "en": "Semangat Orang Muda?", "id": "Darah Muda." },
  { "en": "Tidak Memiliki Perasaan?", "id": "Darah Dingin." },
  { "en": "Melakukan Sesuatu Setengah Hati?", "id": "Hangat Tahi Ayam." },
  { "en": "Tidak Tahu Diri?", "id": "Tebal Muka." },
  { "en": "Kehilangan Reputasi?", "id": "Jatuh Muka." },
  { "en": "Sangat Baik Hatinya?", "id": "Berhati Emas." },
  { "en": "Memiliki Niat Buruk?", "id": "Busuk Hati." },
  { "en": "Orang Yang Pemalas?", "id": "Berat Tulang." },
  { "en": "Sangat Miskin?", "id": "Papa Kedana." },
  { "en": "Berpikir Cepat?", "id": "Panjang Akal." },
  { "en": "Pikiran Sangat Sempit?", "id": "Sempit Akal." },
  { "en": "Mengantuk Sekali?", "id": "Berat Mata." },
  { "en": "Suka Menggoda Lawan Jenis?", "id": "Buaya Darat." },
  { "en": "Sangat Mudah Marah?", "id": "Tipis Telinga." },
  { "en": "Sangat Keras Kepala?", "id": "Kepala Batu." },
  { "en": "Tidak Mau Mendengar Nasehat?", "id": "Masuk Telinga Kanan." },
  { "en": "Suka Berbohong?", "id": "Panjang Lidah." },
  { "en": "Sangat Tajam Perkataannya?", "id": "Lidah Setan." },
  { "en": "Tidak Bisa Dipegang Janjinya?", "id": "Lidah Tak Bertulang." },
  { "en": "Sangat Cerewet?", "id": "Banyak Omong." },
  { "en": "Suka Menyebar Fitnah?", "id": "Mulut Berbisa." },
  { "en": "Beristirahat Sejenak?", "id": "Meluruskan Kaki." },
  { "en": "Anak Kandung?", "id": "Darah Daging." },
  { "en": "Hubungan Renggang?", "id": "Perang Dingin." },
  { "en": "Sesuatu Yang Mustahil?", "id": "Jarum Dalam Jerami." },
  { "en": "Pekerjaan Sangat Mudah?", "id": "Membalik Telapak." },
  { "en": "Orang Yang Suka Mengeluh?", "id": "Tukang Mengeluh." },
  { "en": "Pujian Yang Tidak Tulus?", "id": "Basa Basi." },
  { "en": "Menghina Secara Halus?", "id": "Menyindir Halus." },
  { "en": "Sangat Licik Dan Curang?", "id": "Akal Bulus." },
  { "en": "Membalas Perbuatan Jahat?", "id": "Balas Budi." },
  { "en": "Tidak Tahu Berterima Kasih?", "id": "Kacang Lupa Kulit." },
  { "en": "Suka Mengadu Domba?", "id": "Tukang Kompor." },
  { "en": "Orang Yang Naif?", "id": "Lurus Bendul." },
  { "en": "Sangat Suka Pamer?", "id": "Suka Pamer." },
  { "en": "Mencari Ketenaran?", "id": "Cari Panggung." },
  { "en": "Mencari Kesempatan?", "id": "Mencari Peluang." },
  { "en": "Mengambil Keuntungan?", "id": "Ambil Untung." },
  { "en": "Tidak Punya Pendirian?", "id": "Ikut Angin." },
  { "en": "Orang Yang Selalu Setuju?", "id": "Tukang Stempel." },
  { "en": "Penjilat Atau Cari Muka?", "id": "Anak Buah." },
  { "en": "Orang Suruhan?", "id": "Kaki Tangan." },
  { "en": "Menjadi Mata-mata?", "id": "Telinga Kanan." },
  { "en": "Sangat Lapar?", "id": "Cacing Berontak." },
  { "en": "Sangat Haus?", "id": "Kering Kerontang." },
  { "en": "Menjadi Korban Penipuan?", "id": "Kena Tipu." },
  { "en": "Hampir Celaka?", "id": "Nyaris Maut." },
  { "en": "Sangat Beruntung?", "id": "Dewi Fortuna." },
  { "en": "Selalu Sial?", "id": "Bintang Malang." },
  { "en": "Pesta Besar-besaran?", "id": "Pesta Pora." },
  { "en": "Sangat Berhemat?", "id": "Ikat Pinggang." },
  { "en": "Masa Sulit Ekonomi?", "id": "Masa Paceklik." },
  { "en": "Masa Kejayaan?", "id": "Zaman Keemasan." },
  { "en": "Masalah Yang Rumit?", "id": "Benang Kusut." },
  { "en": "Menyelesaikan Masalah?", "id": "Urai Benang." },
  { "en": "Menambah Masalah?", "id": "Tambah Benang." },
  { "en": "Sangat Cemburu?", "id": "Api Cemburu." },
  { "en": "Sangat Rindu?", "id": "Rindu Dendam." },
  { "en": "Cinta Bertepuk Sebelah?", "id": "Cinta Monyet." },
  { "en": "Cinta Pertama?", "id": "Cinta Pertama." },
  { "en": "Patah Hati?", "id": "Patah Hati." },
  { "en": "Jatuh Cinta?", "id": "Jatuh Hati." },
  { "en": "Orang Tercinta?", "id": "Belahan Jiwa." },
  { "en": "Sangat Setia?", "id": "Setia Sampai Mati." },
  { "en": "Mengkhianati Pasangan?", "id": "Main Serong." },
  { "en": "Menikah?", "id": "Naik Pelaminan." },
  { "en": "Bercerai?", "id": "Turun Ranjang." },
  { "en": "Suka Bertengkar?", "id": "Anjing Dan Kucing." },
  { "en": "Sangat Mesra?", "id": "Dua Sejoli." },
  { "en": "Menjadi Kaya Raya?", "id": "Jadi Jutawan." },
  { "en": "Bangkrut Total?", "id": "Jatuh Miskin." },
  { "en": "Memulai Usaha Baru?", "id": "Buka Usaha." },
  { "en": "Menutup Usaha?", "id": "Tutup Toko." },
  { "en": "Usaha Maju Pesat?", "id": "Laris Manis." },
  { "en": "Usaha Sepi Pelanggan?", "id": "Sepi Pengunjung." },
  { "en": "Banyak Hutang?", "id": "Gali Lubang." },
  { "en": "Bebas Hutang?", "id": "Bebas Utang." },
  { "en": "Sangat Gembira?", "id": "Bunga Hati." },
  { "en": "Menjadi Gila Hormat?", "id": "Mabuk Kuasa." },
  { "en": "Tidak Tahu Sopan Santun?", "id": "Kurang Ajar." },
  { "en": "Berbicara Kasar?", "id": "Mulut Kasar." },
  { "en": "Suka Memerintah?", "id": "Tangan Besi." },
  { "en": "Orang Yang Selalu Mengalah?", "id": "Batu Loncatan." },
  { "en": "Sangat Kuno Atau Ketinggalan Zaman?", "id": "Zaman Batu." },
  { "en": "Sangat Modern Dan Canggih?", "id": "Era Digital." },
  { "en": "Informasi Yang Sangat Cepat?", "id": "Secepat Kilat." },
  { "en": "Sangat Lambat Menerima Informasi?", "id": "Telat Mikir." },
  { "en": "Orang Yang Selalu Update?", "id": "Anak Zaman." },
  { "en": "Orang Yang Gagap Teknologi?", "id": "Gaptek Sekali." },
  { "en": "Sangat Ahli Dalam Suatu Bidang?", "id": "Jago Kandang." },
  { "en": "Tidak Berani Bersaing?", "id": "Jago Kandang." },
  { "en": "Berani Bersaing Di Luar?", "id": "Jago Tandang." },
  { "en": "Orang Yang Banyak Bicara Sedikit Kerja?", "id": "NATO (No Action Talk Only)." },
  { "en": "Orang Yang Sedikit Bicara Banyak Bekerja?", "id": "Air Tenang." },
  { "en": "Sangat Berbahaya Jika Marah?", "id": "Air Tenang Menghanyutkan." },
  { "en": "Orang Yang Terlihat Lemah Tapi Kuat?", "id": "Kecil-kecil Cabai Rawit." },
  { "en": "Orang Yang Terlihat Kuat Tapi Lemah?", "id": "Besar Pasak." },
  { "en": "Pengeluaran Lebih Besar Dari Pendapatan?", "id": "Besar Pasak." },
  { "en": "Hidup Sangat Hemat?", "id": "Ikat Perut." },
  { "en": "Sangat Suka Berfoya-foya?", "id": "Hambur Uang." },
  { "en": "Orang Yang Sangat Cermat?", "id": "Mata Duitan." },
  { "en": "Orang Yang Mudah Ditipu?", "id": "Gampang Dibodohi." },
  { "en": "Sangat Sulit Untuk Ditipu?", "id": "Tidak Bisa Dikadali." },
  { "en": "Terkena Masalah Besar?", "id": "Masuk Angin." },
  { "en": "Sakit Ringan?", "id": "Masuk Angin." },
  { "en": "Mencari Alasan Untuk Tidak Bekerja?", "id": "Cari Alasan." },
  { "en": "Selalu Punya Alasan?", "id": "Seribu Alasan." },
  { "en": "Berbicara Tanpa Fakta?", "id": "Asal Bunyi." },
  { "en": "Berbicara Dengan Data?", "id": "Fakta Bicara." },
  { "en": "Sangat Suka Membaca?", "id": "Kutu Buku." },
  { "en": "Tidak Suka Membaca?", "id": "Malas Baca." },
  { "en": "Wawasan Sangat Luas?", "id": "Jendela Dunia." },
  { "en": "Sangat Berpengetahuan?", "id": "Kamus Berjalan." },
  { "en": "Sangat Lupa Diri?", "id": "Lupa Daratan." },
  { "en": "Sangat Mengingat Budi Baik?", "id": "Ingat Budi." },
  { "en": "Melupakan Kebaikan Orang?", "id": "Lupa Kebaikan." },
  { "en": "Membalas Kejahatan Dengan Kebaikan?", "id": "Air Susu Dibalas." },
  { "en": "Membalas Kebaikan Dengan Kejahatan?", "id": "Air Tuba Dibalas." },
  { "en": "Orang Yang Tidak Tahu Berterima Kasih?", "id": "Kacang Lupa Kulitnya." },
  { "en": "Orang Yang Sangat Sederhana?", "id": "Apa Adanya." },
  { "en": "Suka Berlebihan Dalam Segala Hal?", "id": "Lebay Sekali." },
  { "en": "Sangat Tenang Dalam Situasi Apapun?", "id": "Tetap Tenang." },
  { "en": "Sangat Mudah Panik?", "id": "Sumbu Pendek." },
  { "en": "Menghadapi Masalah Dengan Senyuman?", "id": "Hadapi Senyum." },
  { "en": "Selalu Mengeluh Saat Ada Masalah?", "id": "Cengeng Sekali." },
  { "en": "Mencari Peluang Dalam Kesempitan?", "id": "Peluang Sempit." },
  { "en": "Menyerah Dalam Kesulitan?", "id": "Putus Harapan." },
  { "en": "Selalu Ada Jalan Keluar?", "id": "Banyak Jalan." },
  { "en": "Merasa Tidak Ada Harapan?", "id": "Dunia Runtuh." },
  { "en": "Mendapat Pencerahan?", "id": "Dapat Ilham." },
  { "en": "Pikiran Menjadi Buntu?", "id": "Otak Buntu." },
  { "en": "Menemukan Ide Cemerlang?", "id": "Lampu Pijar." },
  { "en": "Kehabisan Ide?", "id": "Mati Ide." },
  { "en": "Sangat Bersemangat Untuk Memulai?", "id": "Api Semangat." },
  { "en": "Kehilangan Semangat Di Tengah Jalan?", "id": "Layang-layang Putus." },
  { "en": "Menyelesaikan Apa Yang Dimulai?", "id": "Tuntas Kerja." },
  { "en": "Suka Meninggalkan Pekerjaan?", "id": "Setengah-setengah." },
  { "en": "Sangat Fokus Pada Satu Hal?", "id": "Kacamata Kuda." },
  { "en": "Mudah Teralihkan Perhatiannya?", "id": "Gagal Fokus." },
  { "en": "Punya Banyak Keahlian?", "id": "Serba Bisa." },
  { "en": "Tidak Punya Keahlian Khusus?", "id": "Bukan Ahli." },
  { "en": "Ahli Di Segala Bidang?", "id": "Manusia Langka." },
  { "en": "Menjadi Andalan Dalam Tim?", "id": "Pemain Kunci." },
  { "en": "Menjadi Beban Dalam Tim?", "id": "Batu Beban." },
  { "en": "Suka Bekerja Sama?", "id": "Kerja Tim." },
  { "en": "Suka Bekerja Sendiri?", "id": "Serigala Penyendiri." },
  { "en": "Menjadi Pemimpin Yang Mengayomi?", "id": "Bapak Bangsa." },
  { "en": "Menjadi Pemimpin Yang Ditakuti?", "id": "Tangan Dingin." },
  { "en": "Rakyat Yang Sangat Mencintai Pemimpin?", "id": "Cinta Buta." },
  { "en": "Rakyat Yang Sangat Membenci Pemimpin?", "id": "Benci Sekali." },
  { "en": "Menjatuhkan Pemerintahan?", "id": "Kudeta." },
  { "en": "Mempertahankan Kedaulatan?", "id": "Jaga Kedaulatan." },
  { "en": "Menjual Negara?", "id": "Jual Negara." },
  { "en": "Mengabdi Tanpa Batas?", "id": "Abdi Negara." },
  { "en": "Mengorbankan Segalanya Demi Negara?", "id": "Pahlawan Kesiangan." },
  { "en": "Mengaku Sebagai Pahlawan?", "id": "Pahlawan Kesiangan." },
  { "en": "Pahlawan Yang Tidak Dikenal?", "id": "Pahlawan Tanpa Tanda." },
  { "en": "Orang Yang Sangat Berjasa?", "id": "Sangat Berjasa." },
  { "en": "Orang Yang Selalu Merusak?", "id": "Tangan Perusak." },
  { "en": "Membangun Peradaban?", "id": "Bangun Peradaban." },
  { "en": "Menghancurkan Peradaban?", "id": "Hancurkan Peradaban." },
  { "en": "Meninggalkan Warisan Baik?", "id": "Warisan Baik." },
  { "en": "Meninggalkan Warisan Buruk?", "id": "Warisan Buruk." },
  { "en": "Menjadi Sumber Inspirasi?", "id": "Sumber Inspirasi." },
  { "en": "Menjadi Contoh Buruk?", "id": "Contoh Buruk." },
  { "en": "Menyebarkan Kebaikan?", "id": "Tebar Kebaikan." },
  { "en": "Menyebarkan Kebencian?", "id": "Tebar Kebencian." },
  { "en": "Menyatukan Perbedaan?", "id": "Jembatan Perbedaan." },
  { "en": "Memperuncing Perbedaan?", "id": "Gunting Dalam Lipatan." },
  { "en": "Mencari Persamaan?", "id": "Cari Titik Temu." },
  { "en": "Mencari Perbedaan?", "id": "Cari Beda." },
  { "en": "Berpikir Positif?", "id": "Pikiran Positif." },
  { "en": "Berpikir Negatif?", "id": "Pikiran Negatif." },
  { "en": "Selalu Berprasangka Baik?", "id": "Baik Sangka." },
  { "en": "Selalu Berprasangka Buruk?", "id": "Buruk Sangka." },
  { "en": "Melihat Dunia Dengan Optimis?", "id": "Kacamata Merah." },
  { "en": "Melihat Dunia Dengan Pesimis?", "id": "Kacamata Hitam." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
