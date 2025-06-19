<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gestionnaire de Chansons et Accords</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: #333;
            min-height: 100vh;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        header {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 30px;
            text-align: center;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        h1 {
            color: white;
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .subtitle {
            color: rgba(255, 255, 255, 0.8);
            font-size: 1.1rem;
        }

        .nav-tabs {
            display: flex;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 5px;
            margin-bottom: 30px;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        .nav-tab {
            flex: 1;
            padding: 15px;
            background: transparent;
            border: none;
            color: white;
            cursor: pointer;
            border-radius: 10px;
            transition: all 0.3s ease;
            font-size: 1rem;
            font-weight: 500;
        }

        .nav-tab.active {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }

        .nav-tab:hover {
            background: rgba(255, 255, 255, 0.15);
        }

        .tab-content {
            display: none;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
            border: 1px solid rgba(255, 255, 255, 0.3);
        }

        .tab-content.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #555;
        }

        input, textarea, select {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 1rem;
            transition: all 0.3s ease;
            background: white;
        }

        input:focus, textarea:focus, select:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        textarea {
            min-height: 120px;
            resize: vertical;
            font-family: 'Courier New', monospace;
            line-height: 1.5;
        }

        .chord-input {
            font-family: 'Courier New', monospace;
            background: #f8f9fa;
        }

        button {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.3);
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(102, 126, 234, 0.4);
        }

        .song-list {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .song-card {
            background: white;
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            transition: all 0.3s ease;
            border: 1px solid #e0e0e0;
        }

        .song-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 25px rgba(0,0,0,0.15);
        }

        .song-title {
            font-size: 1.3rem;
            font-weight: bold;
            color: #333;
            margin-bottom: 8px;
        }

        .song-artist {
            color: #666;
            margin-bottom: 10px;
        }

        .song-key {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            display: inline-block;
            margin-bottom: 10px;
        }

        .song-preview {
            font-family: 'Courier New', monospace;
            font-size: 0.9rem;
            color: #555;
            line-height: 1.4;
            max-height: 60px;
            overflow: hidden;
        }

        .song-actions {
            margin-top: 15px;
            display: flex;
            gap: 10px;
        }

        .btn-small {
            padding: 8px 15px;
            font-size: 0.9rem;
            background: #28a745;
        }

        .btn-edit {
            background: #ffc107;
            color: #333;
        }

        .btn-delete {
            background: #dc3545;
        }

        .calendar {
            background: white;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .calendar-header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            text-align: center;
            font-size: 1.2rem;
            font-weight: 600;
        }

        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
        }

        .calendar-day {
            aspect-ratio: 1;
            border: 1px solid #e0e0e0;
            padding: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
        }

        .calendar-day:hover {
            background: #f8f9fa;
        }

        .calendar-day.has-event {
            background: linear-gradient(135deg, rgba(102, 126, 234, 0.1) 0%, rgba(118, 75, 162, 0.1) 100%);
            border-color: #667eea;
        }

        .event-dot {
            width: 8px;
            height: 8px;
            background: #667eea;
            border-radius: 50%;
            margin: 2px;
            display: inline-block;
        }

        .section-label {
            font-weight: bold;
            color: #667eea;
            margin-bottom: 5px;
            text-transform: uppercase;
            font-size: 0.9rem;
        }

        .chord-line {
            color: #d63384;
            font-weight: bold;
            font-family: 'Courier New', monospace;
            margin-bottom: 2px;
        }

        .lyric-line {
            color: #333;
            font-family: 'Courier New', monospace;
            margin-bottom: 10px;
        }

        .transposition-controls {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 10px;
            margin: 15px 0;
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .key-display {
            font-size: 1.2rem;
            font-weight: bold;
            color: #667eea;
        }

        .transpose-btn {
            padding: 8px 12px;
            font-size: 0.9rem;
            min-width: 40px;
        }

        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
            backdrop-filter: blur(5px);
        }

        .modal-content {
            background: white;
            margin: 50px auto;
            padding: 30px;
            border-radius: 20px;
            width: 90%;
            max-width: 1200px;
            max-height: 80vh;
            overflow-y: auto;
            box-shadow: 0 20px 40px rgba(0,0,0,0.2);
        }

        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
            transition: color 0.3s ease;
        }

        .close:hover {
            color: #333;
        }

        .event-form {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 20px;
        }

        .song-selector {
            max-height: 200px;
            overflow-y: auto;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            padding: 10px;
        }

        .song-option {
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            transition: background 0.3s ease;
        }

        .song-option:hover {
            background: #e9ecef;
        }

        .song-option.selected {
            background: #667eea;
            color: white;
        }

        .search-box {
            margin-bottom: 20px;
        }

        .chord-suggestions {
            background: #f8f9fa;
            border: 1px solid #e0e0e0;
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 20px;
        }

        .chord-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(60px, 1fr));
            gap: 10px;
        }

        .chord-btn {
            padding: 8px;
            background: white;
            border: 2px solid #667eea;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-family: 'Courier New', monospace;
            font-weight: bold;
            color: #667eea;
        }

        .chord-btn:hover {
            background: #667eea;
            color: white;
        }

        .format-help {
            background: #e7f3ff;
            border-left: 4px solid #2196F3;
            padding: 15px;
            margin: 15px 0;
            border-radius: 0 10px 10px 0;
        }

        .help-title {
            font-weight: bold;
            color: #1976D2;
            margin-bottom: 10px;
        }

        .help-example {
            font-family: 'Courier New', monospace;
            background: rgba(255,255,255,0.7);
            padding: 10px;
            border-radius: 5px;
            margin: 5px 0;
        }

        /* Ajout pour l'impression PDF */
        @media print {
            body, .modal-content, #songDisplay {
                margin: 0 !important;
                padding: 0 !important;
                background: white !important;
            }
            .chord-line, .lyric-line, .section-separator, .section-block {
                page-break-inside: avoid !important;
                break-inside: avoid !important;
            }
            #songDisplay {
                column-count: 2;
                column-gap: 40px;
            }
        }
        .pdf-exporting #songDisplay {
            margin-top: 0 !important;
            padding-top: 0 !important;
            column-count: 2;
            column-gap: 40px;
        }
        .pdf-exporting .chord-line,
        .pdf-exporting .lyric-line,
        .pdf-exporting .section-separator,
        .pdf-exporting .section-block {
            page-break-inside: avoid !important;
            break-inside: avoid !important;
        }

        #songDisplay {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px 20px 0 20px;
            box-sizing: border-box;
        }

        .chord-lyrics-block {
            break-inside: avoid;
            page-break-inside: avoid;
            display: block;
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
</head>
<body>
    <div class="container">
        <header>
            <h1>🎵 HIRA FIANGONANA BATISTA AMBOHITRARAHABA</h1>
            <p class="subtitle">Ahitana ireo hira hirain'ny Fiangonana Batista Ambohitrarahaba</p>
            <div id="songCounter" style="margin-top:10px; color:#fff; font-weight:bold; font-size:1.1rem;"></div>
        </header>

        <div class="nav-tabs">
            <button class="nav-tab active" onclick="showTab('songs')">📚 HIRA</button>
            <button class="nav-tab" onclick="showTab('myown')">⭐ Hamorona ny hirako manokana</button>
            <button class="nav-tab" onclick="showTab('create')">✍️ Hananampy hira</button>
            <button class="nav-tab" onclick="showTab('stats')">📊 Statistiques</button>
            <button class="nav-tab" onclick="showTab('culte')">⛪ Créer un culte</button>
        </div>

        <!-- Onglet Liste des Chansons -->
        <div id="songs-tab" class="tab-content active">
            <div class="search-box">
                <div style="display:flex; gap:10px; margin-bottom:10px;">
                    <button type="button" class="filter-tab active" id="filter-artist" onclick="setSongFilter('artist')">Karazan-kira</button>
                    <button type="button" class="filter-tab" id="filter-title" onclick="setSongFilter('title')">Hira rehetra</button>
                </div>
                <input type="text" id="searchSongs" placeholder="🔍 Rechercher..." oninput="filterSongs()">
            </div>
            <!-- Ajout des boutons d'export/import -->
            <div style="margin:10px 0; display:flex; gap:10px;">
                <button type="button" onclick="exportSongs()" style="background:#1976D2;">⬇️ Hitahiry ny hira</button>
                <button type="button" onclick="document.getElementById('importSongsInput').click()" style="background:#a5a728;">⬆️ Hampiditra hira</button>
                <input type="file" id="importSongsInput" accept=".json" style="display:none" onchange="importSongs(event)">
            </div>
            <div id="songsList" class="song-list"></div>
        </div>

        <!-- Onglet Création de Chanson -->
        <div id="create-tab" class="tab-content">
            <div class="format-help">
                <div class="help-title">💡 Endriky ny "accord"</div>
                <p>Ampiasao ity fombafanoratra ity raha te hanoratra hira sy accord :</p>
                <div class="help-example">[SECTION: Couplet 1]<br>[C]Ohatra ny [Am]amin'ny hira [F]sy accords [G]<br>[C/E]Accord C miampy basse E</div>
                <p>Ireo "Sections" misy : Couplet, Refrain, Bridge, Intro, Outro</p>
            </div>

            <div style="margin-bottom:15px;">
                <button type="button" onclick="changeEditCode()" style="background:#1976D2;">🔑 Raha te hanova ny "code"</button>
            </div>

            
            <form id="songForm">
                <div class="form-group">
                    <label for="songTitle">Lohatenin'ny hira</label>
                    <input type="text" id="songTitle" required autocomplete="off" autocorrect="off" spellcheck="false">
                </div>
                
                <div class="form-group">
                    <label for="songArtist">Karazany</label>
                    <input type="text" id="songArtist" autocomplete="off" autocorrect="off" spellcheck="false">
                </div>
                
                <div class="form-group">
                    <label for="songKey">Tonalité</label>
                    <select id="songKey">
                        <option value="C">C</option>
                        <option value="G">G</option>
                        <option value="D">D</option>
                        <option value="A">A</option>
                        <option value="E">E</option>
                        <option value="B">B</option>
                        <option value="F#">F#</option>
                        <option value="C#">C#</option>
                        <option value="F">F</option>
                        <option value="Bb">Bb</option>
                        <option value="Eb">Eb</option>
                        <option value="Ab">Ab</option>
                        <option value="Db">Db</option>
                        <option value="Gb">Gb</option>
                        <option value="Cb">Cb</option>
                    </select>
                </div>
                
                <div class="chord-suggestions">
                    <strong>Ireo "Accords" misy ato :</strong>
                    <div id="chordGrid"></div>
                </div>
    
                <div class="form-group">
                    <label for="songLyrics">Tononkira sy ireo "accords"</label>
                    <textarea id="songLyrics" class="chord-input"
                        autocomplete="off" autocorrect="off" spellcheck="false"
                        placeholder="[SECTION: Intro]
[C] [Am] [F] [G]

[SECTION: Couplet 1]
[C]Tahaka an'i[Am]zao no
[F]hanaovana [G]azy 

[SECTION: Refrain]
[C]Eto no [Am]soratana 
[F]Ny [G]refrain"></textarea>
                </div>
                
                <button type="submit">💾 Hitahiry ny hira</button>
            </form>
        </div>

        <!-- Onglet Créer ma propre chanson -->
        <div id="myown-tab" class="tab-content">
            <div class="format-help">
                <div class="help-title">⭐ Hamorona ny hiranao manokana</div>
            </div>
            <div class="format-help">
                <div class="help-title">💡 Endriky ny "accord"</div>
                <p>Ampiasao ity fombafanoratra ity raha te hanoratra hira sy accord :</p>
                <div class="help-example">[SECTION: Couplet 1]<br>[C]Ohatra ny [Am]amin'ny hira [F]sy accords [G]<br>[C/E]Accord C miampy basse E</div>
                <p>Ireo "Sections" misy : Couplet, Refrain, Bridge, Intro, Outro</p>
            </div>

            <div class="chord-suggestions">
                <strong>Ireo "accords" izay misy :</strong>
                <div id="myOwnChordGrid"></div>
            </div>
            <form id="myOwnSongForm">
                <div class="form-group">
                    <label for="myOwnSongTitle">Lohatenin'ny hira</label>
                    <input type="text" id="myOwnSongTitle" required autocomplete="off" autocorrect="off" spellcheck="false">
                </div>
                <div class="form-group">
                    <label for="myOwnSongArtist">Karazany</label>
                    <input type="text" id="myOwnSongArtist" autocomplete="off" autocorrect="off" spellcheck="false">
                </div>
                <div class="form-group">
                    <label for="myOwnSongKey">Tonalité</label>
                    <select id="myOwnSongKey">
                        <option value="C">C</option>
                        <option value="G">G</option>
                        <option value="D">D</option>
                        <option value="A">A</option>
                        <option value="E">E</option>
                        <option value="B">B</option>
                        <option value="F#">F#</option>
                        <option value="C#">C#</option>
                        <option value="F">F</option>
                        <option value="Bb">Bb</option>
                        <option value="Eb">Eb</option>
                        <option value="Ab">Ab</option>
                        <option value="Db">Db</option>
                        <option value="Gb">Gb</option>
                        <option value="Cb">Cb</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="myOwnSongLyrics">Tononkira sy "accords"</label>
                    <textarea id="myOwnSongLyrics" class="chord-input"
                        autocomplete="off" autocorrect="off" spellcheck="false"
                        placeholder="[SECTION: Intro]
[C] [Am] [F] [G]

[SECTION: Couplet 1]
[C]Tahaka an'i[Am]zao no
[F]hanaovana [G]azy 

[SECTION: Refrain]
[C]Eto no [Am]soratana 
[F]Ny [G]refrain"></textarea>
                </div>
                <button type="submit" id="myOwnSongSaveBtn">💾 Hitahiry ny hira</button>
            </form>
            <h3 style="margin-top:30px;">⭐ Ireo hirako</h3>
            <div id="myOwnSongsList" class="song-list"></div>
        </div>

        <!-- Onglet Statistiques -->
        <div id="stats-tab" class="tab-content">
            <h2>📊 Statistiques des chansons</h2>
            <div id="statsSummary" style="margin-bottom:20px;"></div>
            <div id="statsCalendarContainer" style="max-width:800px;margin:auto;">
                <div id="statsCalendarHeader" style="text-align:center;font-size:1.3rem;font-weight:bold;margin-bottom:10px;"></div>
                <div id="statsCalendar"></div>
            </div>
        </div>

        <!-- Onglet Créer un culte -->
        <div id="culte-tab" class="tab-content">
            <h2>⛪ Créer un culte / événement</h2>
            <form id="culteForm" style="max-width:700px;margin:auto;">
                <div class="form-group">
                    <label for="culteTitle">Titre du culte / événement</label>
                    <input type="text" id="culteTitle" required>
                </div>
                <div class="form-group">
                    <label for="culteDate">Date</label>
                    <input type="date" id="culteDate" required>
                </div>
                <div class="form-group">
                    <label for="culteDescription">À propos (description, thème, etc.)</label>
                    <textarea id="culteDescription" rows="3"></textarea>
                </div>
                <div class="form-group">
                    <label for="culteSongs">Sélectionner les chansons à jouer</label>
                    <button type="button" onclick="openCulteSongModal()" style="background:#1976D2;margin-bottom:10px;">🎶 Sélectionner les chansons</button>
                    <div id="culteSelectedSongsList" style="margin-top:10px;"></div>
                </div>
                <button type="button" onclick="generateCultePdf()" style="background:#764ba2;">📄 Générer le livret PDF</button>
            </form>
        </div>

        <!-- Modal sélection des chansons pour le culte -->
        <div id="culteSongModal" class="modal">
            <div class="modal-content" style="max-width:900px;">
                <span class="close" onclick="closeCulteSongModal()">&times;</span>
                <h3>🎶 Sélectionner les chansons pour le culte</h3>
                <input type="text" id="culteSongSearch" placeholder="🔍 Rechercher une chanson..." style="width:100%;margin-bottom:10px;padding:10px;border-radius:8px;border:1px solid #e0e0e0;">
                <div id="culteSongsList" style="max-height:350px;overflow-y:auto;border:1px solid #e0e0e0;border-radius:10px;padding:0;background:#f8f9fa;"></div>
                <div style="text-align:right;margin-top:15px;">
                    <button type="button" onclick="validateCulteSongs()" style="background:#28a745;">Valider la sélection</button>
                </div>
            </div>
        </div>

    </div>

    <!-- Modal pour visualiser les chansons -->
    <div id="songModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeSongModal()">&times;</span>
            <div style="text-align:right; margin-bottom:10px;">
                <button class="btn-small" id="downloadSongPdfBtn" onclick="downloadSongPdf()" style="display:none;">📄 Hamadika ho "PDF"</button>
            </div>
            <div id="songDisplay"></div>
        </div>
    </div>

    <script>
        // Variables globales
        let songs = [];
        let mySongs = [];
        let currentDate = new Date();
        let editingSongId = null;
        let editingMyOwnSongId = null;
        let songFilterType = 'artist'; // 'artist' ou 'title'
        let selectedArtist = null;

        // --- Gestion du code de sécurité ---
        function getEditCode() {
            return localStorage.getItem('hirabatista_edit_code') || '1234';
        }
        function setEditCode(newCode) {
            localStorage.setItem('hirabatista_edit_code', newCode);
        }
        function changeEditCode() {
            const oldCode = prompt("Hampidiro ilay code taloha :");
            if (oldCode === null) return; // Annulation
            if (oldCode !== getEditCode()) {
                alert("diso ilay codinao taloha.");
                return;
            }
            const newCode = prompt("Hampidiro ilay code vaovao:");
            if (newCode === null) return; // Annulation
            if (!newCode || newCode.length < 3) {
                alert("Fohy loatra ny code-nao.");
                return;
            }
            const confirmCode = prompt("Hamarino fanindroany :");
            if (confirmCode === null) return; // Annulation
            if (newCode !== confirmCode) {
                alert("Tsy mifanaraka ireo code-nao.");
                return;
            }
            setEditCode(newCode);
            alert("vita soa aman-tsara !");
        }

        // Liste des notes pour chaque type de tonalité (cycle des quintes)
        const NOTES_SHARP = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B'];
        const NOTES_FLAT  = ['C', 'Db', 'D', 'Eb', 'E', 'F', 'Gb', 'G', 'Ab', 'A', 'Bb', 'B'];

        // Pour chaque tonalité, indiquer si elle utilise des dièses ou des bémols
        const KEY_ACCIDENTAL_TYPE = {
            'C': 'sharp',
            'G': 'sharp',
            'D': 'sharp',
            'A': 'sharp',
            'E': 'sharp',
            'B': 'sharp',
            'F#': 'sharp',
            'C#': 'sharp',
            'F': 'flat',
            'Bb': 'flat',
            'Eb': 'flat',
            'Ab': 'flat',
            'Db': 'flat',
            'Gb': 'flat',
            'Cb': 'flat'
        };

        // Map pour normaliser les noms (Db -> C#, etc.)
        const NOTE_EQUIV = {
            'Db': 'C#', 'Eb': 'D#', 'Gb': 'F#', 'Ab': 'G#', 'Bb': 'A#',
            'Cb': 'B',  
        };
        const NOTE_EQUIV_FLAT = {
            'C#': 'Db', 'D#': 'Eb',  'G#': 'Ab', 'A#': 'Bb',
            'B': 'Cb',  
        };

        function normalizeNote(note) {
            return NOTE_EQUIV[note] || note;
        }
        function normalizeNoteFlat(note) {
            // Pour affichage bémol
            for (const [k, v] of Object.entries(NOTE_EQUIV)) {
                if (v === note) return k;
            }
            return note;
        }

        function getAccidentalTypeForKey(key) {
            // Retourne 'sharp' ou 'flat'
            return KEY_ACCIDENTAL_TYPE[key] || 'sharp';
        }

        function transposeNote(note, steps, accidentalType = 'sharp') {
            let arr = accidentalType === 'flat' ? NOTES_FLAT : NOTES_SHARP;
            let idx = arr.indexOf(note);
            // Si la note n'est pas trouvée, essayer de normaliser
            if (idx === -1) {
                note = accidentalType === 'flat' ? normalizeNoteFlat(note) : normalizeNote(note);
                idx = arr.indexOf(note);
            }
            if (idx === -1) return note;
            let newIdx = (idx + steps + 12) % 12;
            return arr[newIdx];
        }

        function transposeChord(chord, semitones) {
            // Gère les slash chords
            if (chord.includes('/')) {
                const [main, bass] = chord.split('/');
                return transposeChord(main, semitones) + '/' + transposeChord(bass, semitones);
            }
            // Extrait la note de base et le reste (enrichissements)
            const regex = /^([A-G][b#]?)(.*)$/;
            const match = chord.match(regex);
            if (!match) return chord;
            const base = match[1];
            const rest = match[2];
            // Déterminer la tonalité cible pour choisir dièse/bémol
            let accidentalType = 'sharp';
            // Si une tonalité cible est affichée, l'utiliser
            let key = document.getElementById('currentKey')?.textContent || window.currentSong?.key || 'C';
            accidentalType = getAccidentalTypeForKey(key);
            let transposed = transposeNote(base, semitones, accidentalType);
            return transposed + rest;
        }

        function transposeLyrics(lyrics, semitones) {
            // Remplace chaque [Accord] par le transposé (y compris slash chords)
            return lyrics.replace(/\[([A-G][b#]?(?:m|maj|min|dim|aug|sus|add)?[0-9#b]*(?:\/[A-G][b#]?)?)\]/g, (m, chord) => {
                return '[' + transposeChord(chord, semitones) + ']';
            });
        }

        function transposeSong(semitones) {
            window.currentTranspose += semitones;
            // Déterminer la tonalité d'origine et la nouvelle tonalité
            const keys_sharp = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B'];
            const keys_flat  = ['C', 'Db', 'D', 'Eb', 'E', 'F', 'Gb', 'G', 'Ab', 'A', 'Bb', 'B'];
            const origKey = window.currentSong.key;
            let accidentalType = getAccidentalTypeForKey(origKey);
            let keys = accidentalType === 'flat' ? keys_flat : keys_sharp;
            let originalKeyIndex = keys.indexOf(origKey);
            // Si la tonalité d'origine n'est pas trouvée, fallback sur sharp
            if (originalKeyIndex === -1) {
                keys = keys_sharp;
                originalKeyIndex = keys.indexOf(origKey);
            }
            let newKeyIndex = (originalKeyIndex + window.currentTranspose + 12) % 12;
            if (newKeyIndex < 0) newKeyIndex += 12;
            let newKey = keys[newKeyIndex];
            // Si la nouvelle tonalité préfère bémols, afficher le nom bémol
            let newAccidentalType = getAccidentalTypeForKey(newKey);
            if (newAccidentalType === 'flat' && NOTE_EQUIV_FLAT[newKey]) {
                newKey = NOTE_EQUIV_FLAT[newKey];
            }
            document.getElementById('currentKey').textContent = newKey;
            const transposedLyrics = transposeLyrics(window.currentSong.lyrics, window.currentTranspose);
            document.getElementById('songContent').innerHTML = formatSongDisplay(transposedLyrics, newKey, window.currentTranspose);
        }

        // --- Affichage accords au-dessus des paroles (alignement précis, espaces intro tous accords) ---
        function formatSongDisplay(lyrics, key, transpose) {
            const lines = lyrics.split('\n');
            let html = '';
            let inIntro = false;
            let i = 0;
            while (i < lines.length) {
                const line = lines[i];
                if (line.startsWith('[SECTION:')) {
                    const sectionName = line.match(/\[SECTION:\s*(.*?)\]/)[1];
                    // Affiche juste le label sans cadre
                    let nextLine = lines[i + 1] || '';
                    let blockHtml = `<div class="section-label">${sectionName}</div>`;
                    inIntro = /intro/i.test(sectionName);
                    // Générer la ligne suivante (accords ou paroles ou vide)
                    if (nextLine) {
                        if (inIntro && /\[([^\]]+)\]/.test(nextLine)) {
                            let chords = [];
                            let lyricsLine = nextLine.replace(/\[([^\]]+)\]/g, '');
                            let match;
                            const regex = /\[([^\]]+)\]/g;
                            while ((match = regex.exec(nextLine)) !== null) {
                                chords.push(match[1]);
                            }
                            let chordsLine = chords.map(c => c).join('  ');
                            blockHtml += `<div class="chord-lyrics-block"><div class="chord-line" style="white-space:pre;">${chordsLine}</div><div class="lyric-line" style="white-space:pre;">${lyricsLine}</div></div>`;
                        } else if (/\[([^\]]+)\]/.test(nextLine)) {
                            let chordsLine = [];
                            let lyricsLine = '';
                            let j = 0;
                            while (j < nextLine.length) {
                                if (nextLine[j] === '[') {
                                    let end = nextLine.indexOf(']', j);
                                    if (end !== -1) {
                                        const chord = nextLine.slice(j + 1, end);
                                        let lyricPos = lyricsLine.length;
                                        for (let k = 0; k < chord.length; k++) {
                                            chordsLine[lyricPos + k] = chord[k];
                                        }
                                        j = end + 1;
                                    } else {
                                        lyricsLine += nextLine[j];
                                        j++;
                                    }
                                } else {
                                    lyricsLine += nextLine[j];
                                    if (typeof chordsLine[lyricsLine.length - 1] === "undefined") {
                                        chordsLine[lyricsLine.length - 1] = ' ';
                                    }
                                    j++;
                                }
                            }
                            for (let k = 0; k < lyricsLine.length; k++) {
                                if (typeof chordsLine[k] === "undefined") chordsLine[k] = ' ';
                            }
                            blockHtml += `<div class="chord-lyrics-block"><div class="chord-line" style="white-space:pre;">${chordsLine.join('')}</div><div class="lyric-line" style="white-space:pre;">${lyricsLine}</div></div>`;
                        } else if (nextLine.trim()) {
                            blockHtml += `<div class="lyric-line">${nextLine}</div>`;
                        } else {
                            blockHtml += '<br>';
                        }
                        i++; // On saute la ligne suivante car elle est déjà traitée
                    }
                    html += `<div class="section-block">${blockHtml}</div>`;
                } else if (line.trim() === '') {
                    html += '<br>';
                } else if (/\[([^\]]+)\]/.test(line)) {
                    let chordsLine = [];
                    let lyricsLine = '';
                    let j = 0;
                    while (j < line.length) {
                        if (line[j] === '[') {
                            let end = line.indexOf(']', j);
                            if (end !== -1) {
                                const chord = line.slice(j + 1, end);
                                let lyricPos = lyricsLine.length;
                                for (let k = 0; k < chord.length; k++) {
                                    chordsLine[lyricPos + k] = chord[k];
                                }
                                j = end + 1;
                            } else {
                                lyricsLine += line[j];
                                j++;
                            }
                        } else {
                            lyricsLine += line[j];
                            if (typeof chordsLine[lyricsLine.length - 1] === "undefined") {
                                chordsLine[lyricsLine.length - 1] = ' ';
                            }
                            j++;
                        }
                    }
                    for (let k = 0; k < lyricsLine.length; k++) {
                        if (typeof chordsLine[k] === "undefined") chordsLine[k] = ' ';
                    }
                    html += `<div class="chord-lyrics-block"><div class="chord-line" style="white-space:pre;">${chordsLine.join('')}</div><div class="lyric-line" style="white-space:pre;">${lyricsLine}</div></div>`;
                } else {
                    html += `<div class="lyric-line">${line}</div>`;
                }
                i++;
            }
            return html;
        }

        // Génère dynamiquement les accords pour chaque tonalité selon le cycle des quintes
        function generateChordsByKey() {
            const chordsByKey = {};
            // Dièses
            ['C', 'G', 'D', 'A', 'E', 'B', 'F#', 'C#'].forEach(key => {
                chordsByKey[key] = [];
                NOTES_SHARP.forEach(note => {
                    chordsByKey[key].push(note);
                    chordsByKey[key].push(note + 'm');
                    chordsByKey[key].push(note + '7');
                    chordsByKey[key].push(note + 'm7');
                    chordsByKey[key].push(note + 'maj7');
                    chordsByKey[key].push(note + 'sus4');
                    chordsByKey[key].push(note + 'sus2');
                    chordsByKey[key].push(note + 'dim');
                    chordsByKey[key].push(note + 'aug');
                    // Slash chords
                    NOTES_SHARP.forEach(bass => {
                        if (note !== bass) chordsByKey[key].push(note + '/' + bass);
                    });
                });
            });
            // Bémols
            ['F', 'Bb', 'Eb', 'Ab', 'Db', 'Gb', 'Cb'].forEach(key => {
                chordsByKey[key] = [];
                NOTES_FLAT.forEach(note => {
                    chordsByKey[key].push(note);
                    chordsByKey[key].push(note + 'm');
                    chordsByKey[key].push(note + '7');
                    chordsByKey[key].push(note + 'm7');
                    chordsByKey[key].push(note + 'maj7');
                    chordsByKey[key].push(note + 'sus4');
                    chordsByKey[key].push(note + 'sus2');
                    chordsByKey[key].push(note + 'dim');
                    chordsByKey[key].push(note + 'aug');
                    // Slash chords
                    NOTES_FLAT.forEach(bass => {
                        if (note !== bass) chordsByKey[key].push(note + '/' + bass);
                    });
                });
            });
            return chordsByKey;
        }
        const chordsByKey = generateChordsByKey();

        // Gestion des accords suggérés
        function updateChordSuggestions() {
            const key = document.getElementById('songKey').value;
            const chordGrid = document.getElementById('chordGrid');
            const chords = chordsByKey[key] || chordsByKey['C'];

            // Grouper par note de base (y compris altérations)
            let baseNotes = KEY_ACCIDENTAL_TYPE[key] === 'flat' ? NOTES_FLAT : NOTES_SHARP;
            const grouped = {};
            baseNotes.forEach(note => grouped[note] = []);
            chords.forEach(chord => {
                // Prend la base exacte (C, C#, Db, etc.)
                const base = chord.match(/^([A-G][b#]?)/);
                if (base && grouped[base[1]]) grouped[base[1]].push(chord);
            });

            chordGrid.innerHTML = '';
            baseNotes.forEach(note => {
                const row = document.createElement('div');
                row.style.display = 'flex';
                row.style.flexWrap = 'wrap';
                row.style.marginBottom = '6px';
                const noteChords = grouped[note].slice(0, 12);
                noteChords.forEach(chord => {
                    const btn = document.createElement('button');
                    btn.type = 'button';
                    btn.className = 'chord-btn';
                    btn.textContent = chord;
                    btn.onclick = () => insertChord(chord);
                    row.appendChild(btn);
                });
                for (let i = noteChords.length; i < 12; i++) {
                    const emptyBtn = document.createElement('button');
                    emptyBtn.type = 'button';
                    emptyBtn.className = 'chord-btn';
                    emptyBtn.disabled = true;
                    emptyBtn.style.visibility = 'hidden';
                    row.appendChild(emptyBtn);
                }
                chordGrid.appendChild(row);
            });
        }

        function insertChord(chord) {
            const textarea = document.getElementById('songLyrics');
            const start = textarea.selectionStart;
            const end = textarea.selectionEnd;
            const text = textarea.value;
            
            const newText = text.substring(0, start) + '[' + chord + ']' + text.substring(end);
            textarea.value = newText;
            textarea.focus();
            textarea.setSelectionRange(start + chord.length + 2, start + chord.length + 2);
        }

        // Gestion des accords suggérés pour "Créer ma propre chanson"
        function updateMyOwnChordSuggestions() {
            const key = document.getElementById('myOwnSongKey').value;
            const chordGrid = document.getElementById('myOwnChordGrid');
            const chords = chordsByKey[key] || chordsByKey['C'];

            let baseNotes = KEY_ACCIDENTAL_TYPE[key] === 'flat' ? NOTES_FLAT : NOTES_SHARP;
            const grouped = {};
            baseNotes.forEach(note => grouped[note] = []);
            chords.forEach(chord => {
                const base = chord.match(/^([A-G][b#]?)/);
                if (base && grouped[base[1]]) grouped[base[1]].push(chord);
            });

            chordGrid.innerHTML = '';
            baseNotes.forEach(note => {
                const row = document.createElement('div');
                row.style.display = 'flex';
                row.style.flexWrap = 'wrap';
                row.style.marginBottom = '6px';
                const noteChords = grouped[note].slice(0, 12);
                noteChords.forEach(chord => {
                    const btn = document.createElement('button');
                    btn.type = 'button';
                    btn.className = 'chord-btn';
                    btn.textContent = chord;
                    btn.onclick = () => insertMyOwnChord(chord);
                    row.appendChild(btn);
                });
                for (let i = noteChords.length; i < 12; i++) {
                    const emptyBtn = document.createElement('button');
                    emptyBtn.type = 'button';
                    emptyBtn.className = 'chord-btn';
                    emptyBtn.disabled = true;
                    emptyBtn.style.visibility = 'hidden';
                    row.appendChild(emptyBtn);
                }
                chordGrid.appendChild(row);
            });
        }

        function insertMyOwnChord(chord) {
            const textarea = document.getElementById('myOwnSongLyrics');
            const start = textarea.selectionStart;
            const end = textarea.selectionEnd;
            const text = textarea.value;
            const newText = text.substring(0, start) + '[' + chord + ']' + text.substring(end);
            textarea.value = newText;
            textarea.focus();
            textarea.setSelectionRange(start + chord.length + 2, start + chord.length + 2);
        }

        document.getElementById('songKey').addEventListener('change', updateChordSuggestions);
        document.getElementById('myOwnSongKey').addEventListener('change', updateMyOwnChordSuggestions);

        // --- Sauvegarde persistante ---
        function saveData() {
            const data = {
                songs: songs
            };
            localStorage.setItem('hirabatista_chansons_data_v2', JSON.stringify(data));
            window.appData = data; // pour compatibilité
        }

        function loadData() {
            let data = null;
            try {
                data = JSON.parse(localStorage.getItem('hirabatista_chansons_data_v2'));
            } catch {}
            if (!data && window.appData) data = window.appData;
            if (data) {
                songs = data.songs || [];
            }
        }

        function saveMySongs() {
            localStorage.setItem('hirabatista_my_own_songs', JSON.stringify(mySongs));
        }

        function loadMySongs() {
            try {
                mySongs = JSON.parse(localStorage.getItem('hirabatista_my_own_songs')) || [];
            } catch {
                mySongs = [];
            }
        }

        // --- Compteur de chansons ---
        function updateSongCounter() {
            const counter = document.getElementById('songCounter');
            if (counter) {
                counter.textContent = "Isan'ny hira rehetra: " + songs.length;
            }
        }

        // --- Statistiques ---
        function getStats() {
            let stats = null;
            try {
                stats = JSON.parse(localStorage.getItem('hirabatista_stats')) || {};
            } catch {
                stats = {};
            }
            if (!stats.actions) stats.actions = [];
            return stats;
        }
        function saveStats(stats) {
            localStorage.setItem('hirabatista_stats', JSON.stringify(stats));
        }
        function logStat(action, song) {
            const stats = getStats();
            stats.actions.push({
                action,
                songId: song?.id || null,
                title: song?.title || '',
                date: new Date().toISOString()
            });
            saveStats(stats);
        }

        // Affichage du résumé et calendrier décoré
        function displayStats() {
            const stats = getStats();
            const actions = stats.actions || [];
            // Calculs
            const added = actions.filter(a => a.action === 'add');
            const removed = actions.filter(a => a.action === 'remove');
            const pdfs = actions.filter(a => a.action === 'pdf');
            // Résumé
            document.getElementById('statsSummary').innerHTML = `
                <table style="width:100%;max-width:500px;margin:auto;text-align:left;">
                    <tr><th>Hira nampidirina</th><td>${added.length}</td></tr>
                    <tr><th>Hira voafafa</th><td>${removed.length}</td></tr>
                    <tr><th>Hira natao PDF</th><td>${pdfs.length}</td></tr>
                </table>
            `;
            // Calendrier décoré (mois courant)
            const now = new Date();
            const year = now.getFullYear();
            const month = now.getMonth();
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            const monthNames = [
                "Janoary", "Febroary", "Martsa", "Aprily", "Mey", "Jona",
                "Jolay", "Aogositra", "Septambra", "Oktobra", "Novambra", "Desambra"
            ];
            document.getElementById('statsCalendarHeader').textContent = `${monthNames[month]} ${year}`;

            // Calcul du premier jour de la semaine (0=dimanche)
            const firstDay = new Date(year, month, 1).getDay();
            let calendar = `
            <style>
                .stats-calendar-table {border-collapse:collapse;width:100%;background:#f8f9fa;border-radius:12px;overflow:hidden;}
                .stats-calendar-table th, .stats-calendar-table td {text-align:center;padding:10px;}
                .stats-calendar-table th {background:#667eea;color:#fff;font-weight:600;}
                .stats-calendar-table td {border:1px solid #e0e0e0;vertical-align:top;min-width:60px;height:70px;position:relative;}
                .stats-calendar-table td .day-number {font-weight:bold;font-size:1.1em;margin-bottom:2px;}
                .stats-calendar-table td .stat-add {color:#1976D2;font-size:0.95em;}
                .stats-calendar-table td .stat-rem {color:#dc3545;font-size:0.95em;}
                .stats-calendar-table td .stat-pdf {color:#764ba2;font-size:0.95em;}
                .stats-calendar-table td.today {background:linear-gradient(135deg,#e7f3ff 60%,#fff);}
            </style>
            <table class="stats-calendar-table">
                <tr>
                    <th>Di</th><th>Lu</th><th>Ma</th><th>Me</th><th>Je</th><th>Ve</th><th>Sa</th>
                </tr>
                <tr>
            `;
            let dayOfWeek = firstDay;
            // Cases vides avant le 1er jour
            for (let i = 0; i < firstDay; i++) {
                calendar += `<td></td>`;
            }
            for (let d = 1; d <= daysInMonth; d++) {
                if (dayOfWeek === 7) {
                    calendar += '</tr><tr>';
                    dayOfWeek = 0;
                }
                const dayStr = `${year}-${String(month+1).padStart(2,'0')}-${String(d).padStart(2,'0')}`;
                const addCount = added.filter(a => a.date.startsWith(dayStr)).length;
                const remCount = removed.filter(a => a.date.startsWith(dayStr)).length;
                const pdfCount = pdfs.filter(a => a.date.startsWith(dayStr)).length;
                const isToday = (now.getDate() === d && now.getMonth() === month && now.getFullYear() === year);
                calendar += `<td class="${isToday ? 'today' : ''}">
                    <div class="day-number">${d}</div>
                    <div class="stat-add">+${addCount}</div>
                    <div class="stat-rem">-${remCount}</div>
                    <div class="stat-pdf">PDF:${pdfCount}</div>
                </td>`;
                dayOfWeek++;
            }
            // Cases vides après le dernier jour
            while (dayOfWeek > 0 && dayOfWeek < 7) {
                calendar += `<td></td>`;
                dayOfWeek++;
            }
            calendar += '</tr></table>';
            document.getElementById('statsCalendar').innerHTML = calendar;
        }

        // Initialisation
        document.addEventListener('DOMContentLoaded', function() {
            loadData();
            loadMySongs();
            displaySongs();
            displayMyOwnSongs();
            updateChordSuggestions();
            updateMyOwnChordSuggestions();
            updateSongCounter();
            updateCulteSelectedSongsList();
        });

        // Gestion des onglets
        function showTab(tabName) {
            // Si on veut ouvrir "Créer une Chanson", demander le code avant d'afficher
            if (tabName === 'create') {
                const EDIT_CODE = getEditCode();
                const userCode = prompt("Hampidiro ny code :");
                if (userCode === null) {
                    // Annulation : ne rien faire, ne rien afficher
                    return;
                }
                if (userCode !== EDIT_CODE) {
                    alert("Diso ny code.");
                    return;
                }
            }
            // Masquer tous les onglets
            const tabs = document.querySelectorAll('.tab-content');
            tabs.forEach(tab => tab.classList.remove('active'));
            
            const navTabs = document.querySelectorAll('.nav-tab');
            navTabs.forEach(tab => tab.classList.remove('active'));
            
            // Afficher l'onglet sélectionné
            document.getElementById(tabName + '-tab').classList.add('active');
            event.target.classList.add('active');
            if (tabName === 'myown') displayMyOwnSongs();
            if (tabName === 'stats') displayStats();
            if (tabName === 'culte') showCulteSongsList();
        }

        // Fonction utilitaire pour trier par numéro puis alphabétique
        function songSortKey(song) {
            // Extrait un numéro au début du titre, sinon Infinity
            const match = (song.title || '').match(/^(\d+)/);
            const num = match ? parseInt(match[1], 10) : Infinity;
            // Pour l'ordre alphabétique, on ignore le numéro initial
            const titleAlpha = (song.title || '').replace(/^\d+\s*/, '').toLowerCase();
            return [num, titleAlpha];
        }

        // Gestion des chansons
        document.getElementById('songForm').addEventListener('submit', function(e) {
            e.preventDefault();
            saveSong();
        });

        function saveSong() {
            const song = {
                id: editingSongId || Date.now(),
                title: document.getElementById('songTitle').value,
                artist: document.getElementById('songArtist').value,
                key: document.getElementById('songKey').value,
                lyrics: document.getElementById('songLyrics').value,
                createdAt: editingSongId ? songs.find(s => s.id === editingSongId).createdAt : new Date().toISOString()
            };

            if (editingSongId) {
                const index = songs.findIndex(s => s.id === editingSongId);
                songs[index] = song;
                editingSongId = null;
            } else {
                songs.push(song);
                logStat('add', song);
            }

            saveData();
            displaySongs();
            document.getElementById('songForm').reset();
            updateChordSuggestions();
            updateSongCounter();
            alert('Voatahiry ny hiranao !');
        }

        function createSongCard(song) {
            const songCard = document.createElement('div');
            songCard.className = 'song-card';
            
            const preview = song.lyrics.replace(/\[SECTION:.*?\]/g, '').replace(/\[.*?\]/g, '').substring(0, 100);
            
            songCard.innerHTML = `
                <div class="song-title">${song.title}</div>
                <div class="song-artist">${song.artist || 'Tsy fantatra'}</div>
                <div class="song-key">Tonalité: ${song.key}</div>
                <div class="song-preview">${preview}...</div>
                <div class="song-actions">
                    <button class="btn-small" onclick="viewSong(${song.id})">👁️ Hijery</button>
                    <button class="btn-small btn-edit" onclick="editSong(${song.id})">✏️ Hanova</button>
                    <button class="btn-small btn-delete" onclick="deleteSong(${song.id})">🗑️ Hamafa</button>
                </div>
            `;
            
            return songCard;
        }

        function displaySongs() {
            const songsList = document.getElementById('songsList');
            songsList.innerHTML = '';
            updateSongCounter();

            if (songFilterType === 'artist') {
                if (!selectedArtist) {
                    // Liste alphabétique des artistes
                    const artistSet = new Set();
                    songs.forEach(song => {
                        if (song.artist && song.artist.trim()) artistSet.add(song.artist.trim());
                    });
                    const artists = Array.from(artistSet).sort((a, b) => a.localeCompare(b));
                    artists.forEach(artist => {
                        const btn = document.createElement('button');
                        btn.textContent = artist;
                        btn.className = 'btn-small';
                        btn.style = "width:100%;margin-bottom:10px;text-align:left;";
                        btn.onclick = () => {
                            selectedArtist = artist;
                            displaySongs();
                        };
                        songsList.appendChild(btn);
                    });
                } else {
                    // Liste des chansons de l'artiste sélectionné
                    const backBtn = document.createElement('button');
                    backBtn.textContent = "← Miverina amin'ny karazan-kira";
                    backBtn.className = 'btn-small';
                    backBtn.style = "margin-bottom:15px;background:#667eea;";
                    backBtn.onclick = () => {
                        selectedArtist = null;
                        displaySongs();
                    };
                    songsList.appendChild(backBtn);

                    const artistSongs = songs.filter(song => (song.artist || '').trim() === selectedArtist)
                        .sort((a, b) => {
                            const ka = songSortKey(a), kb = songSortKey(b);
                            return ka[0] - kb[0] || ka[1].localeCompare(kb[1]);
                        });
                    artistSongs.forEach(song => {
                        songsList.appendChild(createSongCard(song));
                    });
                }
            } else {
                // Liste triée par numéro puis alphabétique
                songs.slice().sort((a, b) => {
                    const ka = songSortKey(a), kb = songSortKey(b);
                    return ka[0] - kb[0] || ka[1].localeCompare(kb[1]);
                }).forEach(song => {
                    songsList.appendChild(createSongCard(song));
                });
            }
        }

        function filterSongs() {
            const searchTerm = document.getElementById('searchSongs').value.toLowerCase();
            if (songFilterType === 'artist') {
                if (!selectedArtist) {
                    // Filtre la liste des artistes
                    const artistSet = new Set();
                    songs.forEach(song => {
                        if (song.artist && song.artist.trim()) artistSet.add(song.artist.trim());
                    });
                    let artists = Array.from(artistSet);
                    if (searchTerm) {
                        artists = artists.filter(artist => artist.toLowerCase().includes(searchTerm));
                    }
                    artists.sort((a, b) => a.localeCompare(b));
                    const songsList = document.getElementById('songsList');
                    songsList.innerHTML = '';
                    artists.forEach(artist => {
                        const btn = document.createElement('button');
                        btn.textContent = artist;
                        btn.className = 'btn-small';
                        btn.style = "width:100%;margin-bottom:10px;text-align:left;";
                        btn.onclick = () => {
                            selectedArtist = artist;
                            displaySongs();
                        };
                        songsList.appendChild(btn);
                    });
                } else {
                    // Filtre les chansons de l'artiste sélectionné
                    let artistSongs = songs.filter(song => (song.artist || '').trim() === selectedArtist);
                    if (searchTerm) {
                        artistSongs = artistSongs.filter(song => (song.title || '').toLowerCase().includes(searchTerm));
                    }
                    artistSongs = artistSongs.sort((a, b) => {
                        const ka = songSortKey(a), kb = songSortKey(b);
                        return ka[0] - kb[0] || ka[1].localeCompare(kb[1]);
                    });
                    const songsList = document.getElementById('songsList');
                    songsList.innerHTML = '';
                    const backBtn = document.createElement('button');
                    backBtn.textContent = "← Retour à la liste des artistes";
                    backBtn.className = 'btn-small';
                    backBtn.style = "margin-bottom:15px;background:#667eea;";
                    backBtn.onclick = () => {
                        selectedArtist = null;
                        displaySongs();
                    };
                    songsList.appendChild(backBtn);
                    artistSongs.forEach(song => {
                        songsList.appendChild(createSongCard(song));
                    });
                }
            } else {
                // Filtre toutes les chansons par titre, puis tri par numéro puis alphabétique
                let filteredSongs = songs;
                if (searchTerm) {
                    filteredSongs = songs.filter(song => (song.title || '').toLowerCase().includes(searchTerm));
                }
                const songsList = document.getElementById('songsList');
                songsList.innerHTML = '';
                filteredSongs.slice().sort((a, b) => {
                    const ka = songSortKey(a), kb = songSortKey(b);
                    return ka[0] - kb[0] || ka[1].localeCompare(kb[1]);
                }).forEach(song => {
                    songsList.appendChild(createSongCard(song));
                });
            }
        }

        function viewSong(id) {
            const song = songs.find(s => s.id === id);
            if (!song) return;

            const modal = document.getElementById('songModal');
            const display = document.getElementById('songDisplay');
            document.getElementById('downloadSongPdfBtn').style.display = '';

            display.innerHTML = `
                <h2>${song.title}</h2>
                <p><strong>Karazany:</strong> ${song.artist || 'Tsy fantatra'}</p>
                <div class="transposition-controls">
                    <strong>Tonalité ankehitriny:</strong>
                    <span class="key-display" id="currentKey">${song.key}</span>
                    <button class="transpose-btn" onclick="transposeSong(-1)">♭</button>
                    <button class="transpose-btn" onclick="transposeSong(1)">#</button>
                </div>
                <div id="songContent">${formatSongDisplay(song.lyrics, song.key, 0)}</div>
            `;
            modal.style.display = 'block';
            window.currentSong = song;
            window.currentTranspose = 0;
        }

        function editSong(id) {
            const EDIT_CODE = getEditCode();
            const userCode = prompt("Ampidiro ny code raha hanova ny hira :");
            if (userCode === null) {
                return;
            }
            if (userCode !== EDIT_CODE) {
                alert("Diso ny code-nao.");
                return;
            }
            const song = songs.find(s => s.id === id);
            if (!song) return;

            editingSongId = id;
            document.getElementById('songTitle').value = song.title;
            document.getElementById('songArtist').value = song.artist || '';
            document.getElementById('songKey').value = song.key;
            document.getElementById('songLyrics').value = song.lyrics;
            
            updateChordSuggestions();
            showTab('create');
        }

        function deleteSong(id) {
            const EDIT_CODE = getEditCode();
            const userCode = prompt("Hampidiro ny code raha hamafa ny hira :");
            if (userCode === null) {
                return;
            }
            if (userCode !== EDIT_CODE) {
                alert("Diso ny code ka tsy afaka mamafa ilay hira ianao.");
                return;
            }
            if (confirm('Tena te hamafa ity hira ity ve ianao ?')) {
                const song = songs.find(s => s.id === id);
                songs = songs.filter(s => s.id !== id);
                logStat('remove', song);
                saveData();
                displaySongs();
                updateSongCounter();
            }
        }

        function closeSongModal() {
            document.getElementById('songModal').style.display = 'none';
        }

        function downloadSongPdf() {
            const display = document.getElementById('songDisplay');
            const clone = display.cloneNode(true);
            Array.from(clone.querySelectorAll('button,.btn-small')).forEach(btn => btn.remove());

            document.body.classList.add('pdf-exporting');
            clone.classList.add('pdf-exporting');

            // Appliquer page-break-inside: avoid sur chaque bloc de ligne et section-block
            clone.querySelectorAll('.chord-line, .lyric-line, .section-separator, .section-block').forEach(el => {
                el.style.pageBreakInside = 'avoid';
                el.style.breakInside = 'avoid';
            });

            // Ajoute un espace en haut de chaque nouvelle page (sauf la première)
            const songPages = clone.querySelectorAll('.song-page');
            for (let i = 1; i < songPages.length; i++) {
                const spacer = document.createElement('div');
                spacer.style.height = '3cm';
                songPages[i].insertBefore(spacer, songPages[i].firstChild);
            }

            const opt = {
                margin:       [5, 10, 5, 10],
                filename:     (window.currentSong?.title || 'partition') + '.pdf',
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  { scale: 2, useCORS: true, scrollY: 0 },
                jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' },
                pagebreak:    { mode: ['avoid-all', 'css', 'legacy'] }
            };
            html2pdf().set(opt).from(clone).save().then(() => {
                document.body.classList.remove('pdf-exporting');
            });
            if (window.currentSong) logStat('pdf', window.currentSong);
        }

        function setSongFilter(type) {
            songFilterType = type;
            selectedArtist = null;
            document.getElementById('filter-artist').classList.toggle('active', type === 'artist');
            document.getElementById('filter-title').classList.toggle('active', type === 'title');
            filterSongs();
        }

        // --- Export/Import des chansons ---
        function exportSongs() {
            const data = {
                songs: songs
            };
            const json = JSON.stringify(data, null, 2);
            const blob = new Blob([json], {type: "application/json"});
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'mes_chansons.json';
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
            URL.revokeObjectURL(url);
            alert('Tehirizo tsara fa afaka afindra @ solosaina hafa.');
            updateSongCounter();
        }

        function importSongs(event) {
            const file = event.target.files[0];
            if (!file) return;
            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const imported = JSON.parse(e.target.result);
                    if (imported && Array.isArray(imported.songs)) {
                        const existingIds = new Set(songs.map(s => s.id));
                        let added = 0;
                        imported.songs.forEach(song => {
                            if (!existingIds.has(song.id)) {
                                songs.push(song);
                                added++;
                            }
                        });
                        saveData();
                        displaySongs();
                        alert(added + ' tafiditra soa aman-tsara!');
                        updateSongCounter();
                    } else {
                        alert('Fichier invalide.');
                    }
                } catch {
                    alert('Erreur lors de la lecture du fichier.');
                }
                event.target.value = '';
            };
            reader.readAsText(file);
        }

        // Gestion des chansons personnelles
        document.getElementById('myOwnSongForm').addEventListener('submit', function(e) {
            e.preventDefault();
            saveMyOwnSong();
        });

        function saveMyOwnSong() {
            const song = {
                id: editingMyOwnSongId || Date.now(),
                title: document.getElementById('myOwnSongTitle').value,
                artist: document.getElementById('myOwnSongArtist').value,
                key: document.getElementById('myOwnSongKey').value,
                lyrics: document.getElementById('myOwnSongLyrics').value,
                createdAt: editingMyOwnSongId ? mySongs.find(s => s.id === editingMyOwnSongId).createdAt : new Date().toISOString()
            };

            if (editingMyOwnSongId) {
                const index = mySongs.findIndex(s => s.id === editingMyOwnSongId);
                mySongs[index] = song;
                editingMyOwnSongId = null;
            } else {
                mySongs.push(song);
            }

            saveMySongs();
            displayMyOwnSongs();
            document.getElementById('myOwnSongForm').reset();
            alert('Voatahiry ny hiranao manokana !');
        }

        function createMyOwnSongCard(song) {
            const songCard = document.createElement('div');
            songCard.className = 'song-card';
            
            const preview = song.lyrics.replace(/\[SECTION:.*?\]/g, '').replace(/\[.*?\]/g, '').substring(0, 100);
            
            songCard.innerHTML = `
                <div class="song-title">${song.title}</div>
                <div class="song-artist">${song.artist || 'Tsy fantatra'}</div>
                <div class="song-key">Tonalité: ${song.key}</div>
                <div class="song-preview">${preview}...</div>
                <div class="song-actions">
                    <button class="btn-small" onclick="viewMyOwnSong(${song.id})">👁️ hijery</button>
                    <button class="btn-small btn-edit" onclick="editMyOwnSong(${song.id})">✏️ hanova</button>
                    <button class="btn-small btn-delete" onclick="deleteMyOwnSong(${song.id})">🗑️ hamafa</button>
                </div>
            `;
            
            return songCard;
        }

        function displayMyOwnSongs() {
            const myOwnSongsList = document.getElementById('myOwnSongsList');
            myOwnSongsList.innerHTML = '';

            mySongs.slice().sort((a, b) => {
                const ka = songSortKey(a), kb = songSortKey(b);
                return ka[0] - kb[0] || ka[1].localeCompare(kb[1]);
            }).forEach(song => {
                myOwnSongsList.appendChild(createMyOwnSongCard(song));
            });
        }

        function viewMyOwnSong(id) {
            const song = mySongs.find(s => s.id === id);
            if (!song) return;

            const modal = document.getElementById('songModal');
            const display = document.getElementById('songDisplay');
            document.getElementById('downloadSongPdfBtn').style.display = '';

            display.innerHTML = `
                <h2>${song.title}</h2>
                <p><strong>Karazany:</strong> ${song.artist || 'Tsy fantatra'}</p>
                <div class="transposition-controls">
                    <strong>Tonalité Ankehitriny:</strong>
                    <span class="key-display" id="currentKey">${song.key}</span>
                    <button class="transpose-btn" onclick="transposeSong(-1)">♭</button>
                    <button class="transpose-btn" onclick="transposeSong(1)">#</button>
                </div>
                <div id="songContent">${formatSongDisplay(song.lyrics, song.key, 0)}</div>
            `;
            modal.style.display = 'block';
            window.currentSong = song;
            window.currentTranspose = 0;
        }

        function editMyOwnSong(id) {
            const song = mySongs.find(s => s.id === id);
            if (!song) return;

            editingMyOwnSongId = id;
            document.getElementById('myOwnSongTitle').value = song.title;
            document.getElementById('myOwnSongArtist').value = song.artist || '';
            document.getElementById('myOwnSongKey').value = song.key;
            document.getElementById('myOwnSongLyrics').value = song.lyrics;
            
            showTab('myown');
        }

        function deleteMyOwnSong(id) {
            if (confirm('Te hamafa ny hira ve ianao?')) {
                mySongs = mySongs.filter(s => s.id !== id);
                saveMySongs();
                displayMyOwnSongs();
            }
        }

        // --- Onglet Culte ---
        let culteSelectedSongs = []; // [{id, transpose}]
        function openCulteSongModal() {
            document.getElementById('culteSongModal').style.display = 'block';
            showCulteSongsList();
        }
        function closeCulteSongModal() {
            document.getElementById('culteSongModal').style.display = 'none';
        }

        function showCulteSongsList() {
            const list = document.getElementById('culteSongsList');
            const search = document.getElementById('culteSongSearch');
            if (!list) return;
            // Ajout du listener de recherche
            if (search && !search._listenerAdded) {
                search.addEventListener('input', showCulteSongsList);
                search._listenerAdded = true;
            }
            // Filtrage
            const filter = (search && search.value) ? search.value.toLowerCase() : '';
            // Trie par numéro puis alphabétique
            const filteredSongs = songs.slice().sort((a, b) => {
                const ka = songSortKey(a), kb = songSortKey(b);
                return ka[0] - kb[0] || ka[1].localeCompare(kb[1]);
            }).filter(song =>
                !filter ||
                (song.title && song.title.toLowerCase().includes(filter)) ||
                (song.artist && song.artist.toLowerCase().includes(filter))
            );
            // Affichage horizontal, chaque ligne occupe toute la largeur
            list.innerHTML = '';
            filteredSongs.forEach(song => {
                const id = 'culte-song-' + song.id;
                // Trouver la transpo déjà choisie
                const found = culteSelectedSongs.find(s => s.id == song.id);
                const checked = found ? 'checked' : '';
                const transpose = found ? found.transpose : 0;
                const div = document.createElement('div');
                div.style = "display:flex;align-items:center;padding:8px 0 8px 0;border-bottom:1px solid #e0e0e0;width:100%;";
                div.innerHTML = `
                    <input type="checkbox" id="${id}" value="${song.id}" style="margin-right:12px;transform:scale(1.3);" ${checked}>
                    <label for="${id}" style="flex:1;cursor:pointer;font-size:1.08em;display:flex;align-items:center;">
                        <span style="font-weight:600;">${song.title}</span>
                        <span style="color:#888;font-size:0.98em;margin-left:10px;">${song.artist ? '('+song.artist+')' : ''}</span>
                    </label>
                    <button type="button" onclick="changeCulteTranspose('${song.id}', this)" style="margin-left:10px;background:#ffc107;color:#333;padding:6px 12px;font-size:0.95em;">Transpose: <span class="culte-transpose-val">${transpose >= 0 ? '+' : ''}${transpose}</span></button>
                `;
                // Ajout du gestionnaire d'ordre de clic
                setTimeout(() => {
                    const cb = div.querySelector('input[type=checkbox]');
                    cb.onclick = function(e) {
                        if (cb.checked) {
                            // Ajouter à la fin si pas déjà présent
                            if (!culteSelectedSongs.find(s => s.id == song.id)) {
                                culteSelectedSongs.push({id: song.id, transpose: 0});
                            }
                        } else {
                            // Retirer si décoché
                            culteSelectedSongs = culteSelectedSongs.filter(s => s.id != song.id);
                        }
                        updateCulteSelectedSongsList();
                    };
                }, 0);
                list.appendChild(div);
            });
        }

        function validateCulteSongs() {
            // Ne fait que fermer la modale et mettre à jour l'affichage (ordre déjà géré au clic)
            closeCulteSongModal();
            updateCulteSelectedSongsList();
        }

        function updateCulteSelectedSongsList() {
            const div = document.getElementById('culteSelectedSongsList');
            if (!div) return;
            if (!culteSelectedSongs.length) {
                div.innerHTML = '<em>Aucune chanson sélectionnée.</em>';
                return;
            }
            div.innerHTML = '';
            // Afficher dans l'ordre de culteSelectedSongs (ordre de sélection)
            culteSelectedSongs.forEach(sel => {
                const song = songs.find(s => s.id == sel.id);
                if (!song) return;
                const row = document.createElement('div');
                row.style = "display:flex;align-items:center;gap:10px;margin-bottom:6px;";
                row.innerHTML = `
                    <span style="font-weight:600;">${song.title}</span>
                    <span style="color:#888;font-size:0.98em;">${song.artist ? '('+song.artist+')' : ''}</span>
                    <span style="color:#764ba2;">Transpose: ${sel.transpose >= 0 ? '+' : ''}${sel.transpose}</span>
                    <button type="button" onclick="removeCulteSong('${sel.id}')" style="background:#dc3545;padding:2px 10px;font-size:0.95em;">✖</button>
                `;
                div.appendChild(row);
            });
        }

        function removeCulteSong(id) {
            culteSelectedSongs = culteSelectedSongs.filter(s => s.id != id);
            updateCulteSelectedSongsList();
        }

        function changeCulteTranspose(songId, btn) {
            let found = culteSelectedSongs.find(s => s.id == songId);
            if (!found) {
                // Si pas encore sélectionné, on coche la case
                document.getElementById('culte-song-' + songId).checked = true;
                found = {id: songId, transpose: 0};
                culteSelectedSongs.push(found);
            }
            let val = prompt("Nombre de demi-tons à transposer (ex: 0, +2, -1) :", found.transpose || 0);
            if (val === null) return;
            val = parseInt(val, 10);
            if (isNaN(val)) val = 0;
            found.transpose = val;
            btn.querySelector('.culte-transpose-val').textContent = (val >= 0 ? '+' : '') + val;
        }

        function generateCultePdf() {
            const title = document.getElementById('culteTitle').value.trim();
            const date = document.getElementById('culteDate').value;
            const desc = document.getElementById('culteDescription').value.trim();
            if (!title || !date || !culteSelectedSongs.length) {
                alert("Veuillez remplir le titre, la date et sélectionner au moins une chanson.");
                return;
            }
            // Utiliser l'ordre de culteSelectedSongs pour le PDF
            const selectedSongs = culteSelectedSongs.map(sel => {
                const song = songs.find(s => s.id == sel.id);
                return song ? { ...song, transpose: sel.transpose } : null;
            }).filter(Boolean);

            // CSS pour le PDF : marges hautes augmentées, meta en colonne unique centrée, paroles en deux colonnes
            let pdfStyles = `
                <style>
                    @page { margin: 40mm 15mm 20mm 15mm !important; }
                    .culte-song-block {
                        page-break-inside: avoid;
                        break-inside: avoid;
                        margin-bottom: 30px;
                    }
                    .culte-song-meta-col {
                        text-align: center;
                        margin-bottom: 18px;
                        font-size: 1.08em;
                        color: #764ba2;
                        font-weight: 600;
                    }
                    .culte-song-content-cols {
                        column-count: 2;
                        column-gap: 40px;
                        column-fill: auto;
                        word-break: break-word;
                        hyphens: auto;
                        break-inside: avoid;
                    }
                    .culte-song-block h2 {
                        text-align: center;
                        color: #764ba2;
                        margin-bottom: 0;
                        margin-top: 0;
                        break-inside: avoid;
                    }
                    .chord-lyrics-block {
                        break-inside: avoid;
                        page-break-inside: avoid;
                        display: block;
                    }
                    .culte-page-number {
                        position: fixed;
                        bottom: 10mm;
                        left: 0;
                        right: 0;
                        text-align: center;
                        color: #888;
                        font-size: 0.95em;
                        z-index: 9999;
                    }
                </style>
            `;

            let html = pdfStyles + `
                <div style="text-align:center;margin-bottom:30px;">
                    <h1 style="margin-bottom:10px;color: #111;">${title}</h1>
                    <div style="font-size:1.1em;margin-bottom:8px;">${date}</div>
                    <div style="font-size:1.05em;color:#555;margin-bottom:20px;">${desc.replace(/\n/g,'<br>')}</div>
                </div>
            `;
            selectedSongs.forEach((song, idx) => {
                // Transposer les paroles si besoin
                let lyrics = song.lyrics;
                if (song.transpose && song.transpose !== 0) {
                    lyrics = transposeLyrics(song.lyrics, song.transpose);
                }
                html += `
                    <div class="culte-song-block" style="page-break-before:${idx>0?'always':'auto'};">
                        <h2>${song.title}</h2>
                        <div class="culte-song-meta-col">
                            ${song.artist ? song.artist + ' &mdash; ' : ''}Tonalité: ${song.key}${song.transpose && song.transpose !== 0 ? ' (Transposé ' + (song.transpose > 0 ? '+' : '') + song.transpose + ')' : ''}
                        </div>
                        <div class="culte-song-content-cols">${formatSongDisplay(lyrics, song.key, song.transpose || 0)}</div>
                    </div>
                `;
            });

            const opt = {
                margin:       [40, 15, 20, 15], // top, right, bottom, left (mm)
                filename:     `${title.replace(/\s+/g,'_')}_${date}.pdf`,
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  { scale: 2, useCORS: true, scrollY: 0 },
                jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' },
                pagebreak:    { mode: ['avoid-all', 'css', 'legacy'] }
            };
            const container = document.createElement('div');
            container.innerHTML = html;

            // Génération PDF avec pagination
            html2pdf().set(opt).from(container).toPdf().get('pdf').then(function(pdf) {
                const totalPages = pdf.internal.getNumberOfPages();
                for (let i = 1; i <= totalPages; i++) {
                    pdf.setPage(i);
                    pdf.setFontSize(10);
                    pdf.setTextColor(136,136,136);
                    pdf.text(`Page ${i} / ${totalPages}`, pdf.internal.pageSize.getWidth()/2, pdf.internal.pageSize.getHeight()-10, {align:'center'});
                }
            }).save();
        }
    </script>
</body>
</html>
