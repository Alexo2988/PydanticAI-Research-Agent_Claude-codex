# PydanticAI Research & Email Agent System

Ein produktionsbereites KI-Agent-System, das mit PydanticAI entwickelt wurde und Web-Recherche-Funktionen mit Gmail E-Mail-Entwürfen kombiniert und dabei eine wunderschöne Streaming-CLI-Oberfläche bietet. **Dieses Repository demonstriert auch die automatisierte KI-gestützte Fehlerbehebung und PR-Review mit mehreren KI-Coding-Assistenten.**

## 🚀 Funktionen

- **Web-Recherche**: Nutzt die Brave Search API für aktuelle, relevante Informationen
- **E-Mail-Entwürfe**: Erstellt professionelle Gmail-Entwürfe basierend auf Recherche-Ergebnissen
- **Agent-Delegation**: Der Recherche-Agent delegiert E-Mail-Aufgaben an spezialisierte E-Mail-Agenten
- **Streaming CLI**: Wunderschöne Echtzeit-Ausgabe mit der Rich-Bibliothek und PydanticAI's `.iter()`-Methode
- **OAuth2-Integration**: Sichere Gmail-Authentifizierung mit geführtem Setup-Assistenten
- **Mock-Testing**: Umfassende Testsuite mit TestModel und Mock-Services
- **Produktionsbereit**: Umgebungsbasierte Konfiguration, Fehlerbehandlung und Protokollierung
- **KI-gestützte Workflows**: Automatisierte Fehlerbehebung und PR-Reviews über Claude Code, Codex und Cursor

## 🤖 KI-Coding-Assistenten (GitHub Actions)

Dieses Repository zeigt die automatisierte Issue-Behandlung und Code-Review mit drei führenden KI-Coding-Assistenten. Erwähnen Sie sie einfach in Issue- oder PR-Kommentaren, um automatisierte Workflows auszulösen.

### Verfügbare Befehle

- **Claude Code**: `@claude-fix` oder `@claude-review`
- **OpenAI Codex**: `@codex-fix` oder `@codex-review`
- **Cursor**: `@cursor-fix` oder `@cursor-review`

### Setup-Anweisungen

Fügen Sie die folgenden Secrets zu Ihrem GitHub-Repository hinzu (`Settings → Secrets and variables → Actions → New repository secret`):

1. **Claude Code**: `CLAUDE_CODE_OAUTH_TOKEN`
   - Installieren Sie Claude CLI: `npm install -g @anthropic-ai/claude-code`
   - Token generieren: `claude setup-token` (erstellt einen 1-Jahres-Token beginnend mit `sk-ant-oat01-`)
   - Kopieren Sie den generierten Token
2. **OpenAI Codex**: `OPENAI_API_KEY` - [Von der OpenAI-Plattform erhalten](https://platform.openai.com/api-keys)
3. **Cursor**: `CURSOR_API_KEY` - [Vom Cursor-Dashboard generieren](https://cursor.com/)

### Funktionsweise

Die Workflows verwenden wiederverwendbare Prompt-Vorlagen (`.github/issue_fix_prompt.md` und `.github/pr_review_prompt.md`), die die Fix- und Review-Prozesse definieren. Jeder KI-Assistent-Workflow lädt diese Vorlagen und passt sie mit dem entsprechenden Branch-Namenssuffix an (`-claude`, `-codex` oder `-cursor`). Dies gewährleistet Konsistenz über alle KI-Assistenten hinweg und behält gleichzeitig separate Zuordnungen für Fixes und Reviews bei.

**Workflow-Dateien**:
- `.github/workflows/claude_code/` - Claude Code Workflows
- `.github/workflows/codex/` - OpenAI Codex Workflows
- `.github/workflows/cursor/` - Cursor Workflows

## 🏗️ Pydantic AI Agent Architektur

```
├── agents/          # PydanticAI Agenten
│   ├── research_agent.py    # Haupt-Recherche-Agent mit Brave Search
│   └── email_agent.py       # Gmail-Entwurf-Erstellungs-Agent
├── config/          # Einstellungen und Modell-Provider
│   ├── settings.py          # Umgebungsbasierte Konfiguration
│   └── providers.py         # LLM-Modell-Setup
├── models/          # Pydantic-Datenmodelle
│   ├── email_models.py      # E-Mail-bezogene Modelle
│   ├── research_models.py   # Recherche-Datenmodelle
│   └── agent_models.py      # Generische Agent-Modelle
├── tools/           # Tool-Funktionen
│   ├── brave_search.py      # Brave Search API-Integration
│   └── gmail_tools.py       # Gmail OAuth2 und Entwurf-Erstellung
├── tests/           # Test-Suite
│   ├── test_research_agent.py
│   ├── test_email_agent.py
│   └── pytest.ini
├── gmail_setup.py   # Gmail OAuth2 Setup-Assistent
└── research_email_cli.py  # Haupt-CLI-Anwendung
```

## 📋 Voraussetzungen

1. **Python 3.11+** mit Virtual Environment-Funktionalität
2. **API-Schlüssel**:
   - OpenAI API-Schlüssel (für LLM)
   - Brave Search API-Schlüssel (für Web-Suche)
3. **Gmail OAuth2-Setup**:
   - Google Cloud-Projekt mit aktivierter Gmail API
   - OAuth2-Anmeldedaten von der Google Cloud Console heruntergeladen

## 🛠️ Installation

1. **Klonen und Virtual Environment einrichten**:
   ```bash
   git clone <repository-url>
   cd PydanticAI-Research-Agent
   python3 -m venv venv
   source venv/bin/activate  # Unter Windows: venv\Scripts\activate
   ```

2. **Abhängigkeiten installieren**:
   ```bash
   pip install 'pydantic-ai-slim[openai]' httpx rich python-dotenv
   pip install google-auth google-auth-oauthlib google-api-python-client
   pip install pytest pytest-asyncio  # Für Tests
   ```

3. **Umgebung konfigurieren**:
   ```bash
   cp .env.example .env
   # Bearbeiten Sie .env mit Ihren API-Schlüsseln
   ```

4. **Gmail OAuth2 einrichten**:
   ```bash
   python gmail_setup.py
   ```

## ⚙️ Konfiguration

### Umgebungsvariablen (.env)

```bash
# LLM-Konfiguration
LLM_PROVIDER=openai
LLM_API_KEY=ihr-openai-api-schlüssel-hier
LLM_MODEL=gpt-4o
LLM_BASE_URL=https://api.openai.com/v1

# Brave Search-Konfiguration
BRAVE_API_KEY=ihr-brave-search-api-schlüssel-hier

# Gmail OAuth2-Konfiguration  
GMAIL_CREDENTIALS_PATH=credentials.json
GMAIL_TOKEN_PATH=token.pickle

# Anwendungskonfiguration
APP_ENV=development
LOG_LEVEL=INFO
DEBUG=false
```

### Gmail OAuth2-Setup

1. **Google Cloud-Projekt erstellen**:
   - Gehen Sie zur [Google Cloud Console](https://console.cloud.google.com)
   - Erstellen Sie ein neues Projekt oder wählen Sie ein bestehendes aus

2. **Gmail API aktivieren**:
   - Gehen Sie zu APIs & Services > Library
   - Suchen Sie "Gmail API" und aktivieren Sie es

3. **OAuth2-Anmeldedaten erstellen**:
   - Gehen Sie zu APIs & Services > Credentials
   - Erstellen Sie eine OAuth 2.0 Client-ID für Desktop-Anwendung
   - Als `credentials.json` herunterladen

4. **Setup-Assistenten ausführen**:
   ```bash
   python gmail_setup.py
   ```

## 🎯 Verwendung

### CLI-Schnittstelle

```bash
source venv/bin/activate
python research_email_cli.py
```

### Beispiel-Anfragen

- "Recherchiere KI-Sicherheitstrends und sende eine Zusammenfassung per E-Mail an john@company.com"
- "Finde die neuesten Entwicklungen im Quantencomputing"
- "Erstelle einen E-Mail-Entwurf über Marktanalyse für jane.doe@firm.com"

### Programmatische Verwendung

```python
from agents import research_agent, ResearchAgentDependencies
from config.settings import settings

# Abhängigkeiten erstellen
deps = ResearchAgentDependencies(
    brave_api_key=settings.brave_api_key,
    gmail_credentials_path=settings.gmail_credentials_path,
    gmail_token_path=settings.gmail_token_path
)

# Recherche-Agent ausführen
result = await research_agent.run(
    "Recherchiere Machine Learning-Trends",
    deps=deps
)
```

## 🧪 Tests

Führen Sie die Test-Suite mit pytest aus:

```bash
source venv/bin/activate
python -m pytest tests/ -v
```

### Test-Umgebung

Tests verwenden Mock-Services und TestModel für die Validierung ohne externe API-Aufrufe:

```python
# Test-Modus aktivieren
import os
os.environ['TESTING'] = 'true'

# TestModel für vorhersagbare Antworten verwenden
from pydantic_ai.models.test import TestModel
test_model = TestModel()

with research_agent.override(model=test_model):
    result = research_agent.run_sync("Test-Anfrage", deps=deps)
```

## 🔧 Entwicklung

### Agent-Tools

**Recherche-Agent**:
- `search_web`: Brave Search API-Integration
- `create_email_draft`: Delegiert an E-Mail-Agent
- `summarize_research`: Erstellt strukturierte Zusammenfassungen

**E-Mail-Agent**:
- `authenticate_gmail`: OAuth2-Authentifizierung
- `create_gmail_draft`: Entwurf-Erstellung in Gmail
- `compose_email_content`: Professionelle E-Mail-Komposition

### Fehlerbehandlung

Das System beinhaltet umfassende Fehlerbehandlung:

- **Gmail OAuth2**: Detaillierte Setup-Anleitung und Token-Aktualisierung
- **API-Fehler**: Graceful Degradation und Retry-Mechanismen
- **Netzwerk-Probleme**: Timeout-Behandlung und Verbindungswiederherstellung
- **Benutzer-Führung**: Umsetzbare Fehlermeldungen mit nächsten Schritten

### Sicherheits-Features

- **Umgebungsvariablen**: Keine fest codierten Geheimnisse
- **OAuth2-Flow**: Sichere Gmail-Authentifizierung
- **Input-Validierung**: Pydantic-Modell-Validierung
- **API-Schlüssel-Schutz**: Niemals protokolliert oder in Fehlern preisgegeben

## 📚 Verwendete PydanticAI-Muster

Diese Implementierung demonstriert wichtige PydanticAI-Muster:

- **Agent-Komposition**: Mehrere spezialisierte Agenten arbeiten zusammen
- **Dependency Injection**: Saubere Trennung der Belange mit `deps_type`
- **Tool-Integration**: `@agent.tool`-Dekoratoren mit richtigem Kontext
- **Modell-Override**: TestModel für Entwicklung und Tests
- **Streaming-Ausgabe**: Echtzeit-CLI mit `.iter()`-Methode
- **Usage-Tracking**: Token-Zählung über Agent-Delegationen hinweg
- **Fehler-Wiederherstellung**: Graceful Behandlung von externen Service-Fehlern

## 🚨 Wichtige Hinweise

- **Niemals committen**: `credentials.json` oder `token.pickle` in die Versionskontrolle
- **Zu .gitignore hinzufügen**: Alle sensiblen Dateien sind ordnungsgemäß ausgeschlossen
- **API-Rate-Limits**: Brave Search hat Nutzungsquoten - überwachen Sie den Verbrauch
- **Token-Ablauf**: Gmail-Token werden automatisch aktualisiert, können aber eine erneute Authentifizierung erfordern
- **Mock-Modus**: Setzen Sie `TESTING=true` für Entwicklung ohne echte API-Aufrufe

## 🆘 Fehlerbehebung

### Häufige Probleme

1. **"No module named 'pydantic_ai'"**:
   ```bash
   pip install 'pydantic-ai-slim[openai]'
   ```

2. **Gmail-Authentifizierungsfehler**:
   ```bash
   python gmail_setup.py  # OAuth2-Setup erneut ausführen
   ```

3. **Import-Fehler**:
   ```bash
   source venv/bin/activate  # Sicherstellen, dass Virtual Environment aktiv ist
   ```

4. **Fehlende API-Schlüssel**:
   - Überprüfen Sie, ob die `.env`-Datei existiert und gültige Schlüssel hat
   - Verifizieren Sie, dass Umgebungsvariablen geladen wurden

## 📖 Mehr erfahren

- [PydanticAI-Dokumentation](https://ai.pydantic.dev/)
- [Brave Search API](https://brave.com/search/api/)
- [Gmail API Python Quickstart](https://developers.google.com/workspace/gmail/api/quickstart/python)
- [Rich Library-Dokumentation](https://rich.readthedocs.io/)

## 🤝 Mitwirken

Dieses Projekt folgt PydanticAI-Best-Practices:

- Verwenden Sie umgebungsbasierte Konfiguration
- Implementieren Sie umfassende Fehlerbehandlung
- Fügen Sie TestModel-Validierung für alle Agenten hinzu
- Folgen Sie Agent-zu-Agent-Delegationsmustern
- Halten Sie Sicherheitsstandards für API-Schlüssel und OAuth2 ein

---

Erstellt mit ❤️ unter Verwendung von [PydanticAI](https://ai.pydantic.dev/) und nach produktionsbereiten Entwicklungspraktiken.