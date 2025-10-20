# 💬 Lite Chatbot • Streamlit

A **minimal Streamlit chatbot** built to run entirely within **Google Colab**. This project demonstrates how to host a local Streamlit app using **Cloudflare Tunnel (cloudflared)** — no API keys, no tokens, no external dependencies.

---

## 🚀 Features

* 💬 **Offline chatbot** — runs completely locally in Colab.
* 🎨 **Streamlit-based interface** — intuitive and responsive UI.
* ⚙️ **Configurable settings** — choose UI theme, response style, typing speed, and temperature.
* 🌐 **Cloudflare Tunnel** — instantly share your local Streamlit app via public URL.
* 🧹 **Clear chat history** — reset conversation directly from sidebar.
* 🔒 **Privacy-first** — no data leaves the notebook.

---

## 🧩 Installation & Setup

Run these commands in a **Colab cell** to set up dependencies:

```bash
!pip -q install streamlit==1.36.0
# Download a small static cloudflared binary (no login/token needed)
!wget -q -O cloudflared https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64
!chmod +x cloudflared
```

---

## 📄 Creating the Chatbot App

Write the following code into a file named `app.py`:

```bash
%%writefile app.py
# Full code for the chatbot is included in this repository under app.py
# It handles UI layout, sidebar controls, local toy model logic, and response animations.
```

The chatbot uses a **tiny local language simulation** — it reacts to greetings, tells jokes, echoes responses, and mimics the Eliza chatbot style. The sidebar allows configuration for themes, typing speed, and creativity.

---

## ▶️ Running the App

Launch the Streamlit app and expose it publicly using Cloudflare Tunnel:

```bash
!streamlit run app.py --server.port 8501 --server.address 0.0.0.0 >/dev/null 2>&1 &
!./cloudflared tunnel --url http://localhost:8501 --logfile cloudflared.log --loglevel info >/dev/null 2>&1 &
```

Then, extract your **public app URL**:

```python
import re, time, pathlib
log = pathlib.Path("cloudflared.log")
# Wait up to 30s for the tunnel URL to appear
for _ in range(30):
    if log.exists():
        txt = log.read_text()
        m = re.search(r"https://[a-z0-9-]+\.trycloudflare\.com", txt)
        if m:
            print("Your public app URL:\n", m.group(0))
            break
    time.sleep(1)
else:
    print("Could not detect the tunnel URL yet. Re-run this cell in a few seconds to refresh.")
```

Example output:

```
Your public app URL:
 https://racing-liberty-effect-transmitted.trycloudflare.com
```

---

## 💡 Response Modes

| Mode          | Description                                       |
| ------------- | ------------------------------------------------- |
| **Helpful**   | Provides random advice and ideas.                 |
| **Echo**      | Repeats your input verbatim.                      |
| **Eliza-ish** | Mimics old-school reflective therapist responses. |
| **Concise**   | Responds briefly, ideal for minimal feedback.     |

---

## 📸 Screenshot

Below is the demo screenshot of the running app in Colab.

![UI using Streamlit](UI%20using%20Streamlit.png)

---

## 📜 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.
