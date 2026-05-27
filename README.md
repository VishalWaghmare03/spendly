# Spendly

An **expense tracking application** built with Python and Flask, powered by Claude Code. Spendly helps you manage your personal finances by tracking income and expenses with a clean, intuitive interface.

## Features

- Add, view, and manage income and expense transactions
- Categorize spending for better financial insight
- Dashboard with spending summaries and breakdowns
- Persistent storage using a lightweight database
- Responsive landing page with demo video modal
- Terms of Service and Privacy Policy pages

## Tech Stack

| Layer | Technology |
|-------|------------|
| Backend | Python, Flask (app.py) |
| Frontend | HTML, CSS, JavaScript |
| Database | SQLite (via `/database` module) |
| Styling | Custom CSS (51.7%) |
| Templates | Jinja2 (HTML templates) |

## Project Structure

```
spendly/
├── app.py                  # Main Flask application
├── database/               # Database models and helpers
├── static/                 # CSS, JS, and static assets
├── templates/              # Jinja2 HTML templates
├── requirements.txt        # Python dependencies
└── .gitignore
```

## Getting Started

### Prerequisites

- Python 3.8+
- pip

### Installation

```bash
git clone https://github.com/VishalWaghmare03/spendly.git
cd spendly
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

### Running the App

```bash
python app.py
```

Open your browser at `http://localhost:5000`

## Screenshots

![Spendly Screenshot](Screenshot%202026-03-25%20at%2012.36.20%20AM.png)

## Built With Claude Code

This project was developed using [Claude Code](https://claude.ai/code) by Anthropic — an AI-powered coding assistant.

## License

MIT
