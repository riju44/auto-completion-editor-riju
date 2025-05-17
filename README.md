# auto-completion-editor-riju
import ipywidgets as widgets
from IPython.display import display, HTML, clear_output

# --- Display Logo ---
display(HTML("""
<style>
    body { background-color: white !important; }

    .logo-container {
        text-align: center;
        padding-top: 40px;
        background-color: white;
        font-family: Arial, sans-serif;
    }

    .autocomplete-logo {
        font-size: 48px;
        font-weight: bold;
        margin-bottom: 10px;
    }

    .autocomplete-logo span:nth-child(1) { color: #4285F4; }  /* A */
    .autocomplete-logo span:nth-child(2) { color:#4285F4; }  /* U */
    .autocomplete-logo span:nth-child(3) { color: #4285F4; }  /* T */
    .autocomplete-logo span:nth-child(4) { color: #4285F4; }  /* O */
    .autocomplete-logo span:nth-child(5) { color: #4285F4; }  /* C */
    .autocomplete-logo span:nth-child(6) { color: #4285F4; }  /* O */
    .autocomplete-logo span:nth-child(7) { color: #4285F4; }  /* M */
    .autocomplete-logo span:nth-child(8) { color: #4285F4; }  /* P */
    .autocomplete-logo span:nth-child(9) { color: #4285F4; }  /* L */
    .autocomplete-logo span:nth-child(10) { color: #4285F4; } /* E */
    .autocomplete-logo span:nth-child(11) { color: #4285F4; } /* T */
    .autocomplete-logo span:nth-child(12) { color: #4285F4; } /* I */
    .autocomplete-logo span:nth-child(13) { color: #4285F4; } /* O */
    .autocomplete-logo span:nth-child(14) { color: #4285F4; } /* N */
</style>

<div class="logo-container">
    <div class="autocomplete-logo">
        <span>A</span><span>U</span><span>T</span><span>O</span><span>C</span><span>O</span><span>M</span><span>P</span><span>L</span><span>E</span><span>T</span><span>I</span><span>O</span><span>N</span>
    </div>
    <div style="font-size: 24px; color: #555;">IN TEXT EDITOR</div>
</div>
"""))

# --- Keywords ---
keywords = sorted([
    "Natural Language Processing", "Colab.research.google.com", "Google.com", "Gmail.com",
    "Tamil Movies 2025", "Tamil Songs in 2025", "Tamil Songs in 2008", "Tamil Vadivel comedy",
    "Tamil Gowndamani Senthil comedy", "Moviesda.com", "Tamil Latest Movie", "Irctc.com",
    "GUVI GEEK NETWORK PRIVATE LIMITED", "HCLTech", "Naan Mudhalvan Upskilling Platform",
    "IPL Today Match", "Naan Mudhalvan Upskilling Platform login", "Youtube.com",
    "Java Script", "Java online Compiler", "C++", "Auto completion in Text Editor",
    "Auto completion in Text view" ,"coffee","sathish"
], key=str.lower)

# --- Search Box ---
editor = widgets.Text(
    placeholder='AUTOCOMPLETION IN TEXT EDITORS',
    layout=widgets.Layout(width='100%', height='70px', padding='15px'),
    style={'description_width': '0px'}
)

# --- Apply Styling ---
display(HTML("""
<style>
    .widget-text input {
        background-color: black !important;
        color: white !important;
        font-size: 20px !important;
        border-radius: 40px !important;
        padding: 20px 30px !important;
        border: 3px solid #dfe1e5 !important;
    }
</style>
"""))

# --- Search Layout ---
search_bar = widgets.HBox([editor], layout=widgets.Layout(width='100%', padding='5px'))
suggestions_out = widgets.Output(layout=widgets.Layout(width='100%'))

search_area = widgets.VBox([search_bar, suggestions_out],
    layout=widgets.Layout(width='70%'))

# 🟡 Move Search Box Down
center_box = widgets.VBox([search_area],
    layout=widgets.Layout(
        align_items='center',
        margin='80px 0px 0px 0px'  # top, right, bottom, left
    )
)

# --- Suggestion Logic ---
def update_suggestions(change):
    typed = change['new'].strip().lower()
    suggestions_out.clear_output()
    if typed:
        starts_with = []
        contains = []
        for kw in keywords:
            kw_lower = kw.lower()
            if kw_lower.startswith(typed):
                starts_with.append(kw)
            elif typed in kw_lower:
                contains.append(kw)
        matches = starts_with + contains
        with suggestions_out:
            for match in matches:
                kw_lower = match.lower()
                start = kw_lower.find(typed)
                end = start + len(typed)
                highlighted = (
                    match[:start] +
                    f"<span style='font-weight:bold; color:black; background-color:#f1f3f4;'>{match[start:end]}</span>" +
                    match[end:]
                )
                html = f"""
                <div style='text-align: left; padding: 6px 8px; font-size: 18px; color: #333; display: flex; align-items: center;'>
                    <span style='margin-right: 10px;'>➭</span>
                    {highlighted}
                </div>
                """
                display(widgets.HTML(html))

# --- Bind Input ---
editor.observe(update_suggestions, names='value')

# --- Display Everything ---
display(center_box)
