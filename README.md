# RPSC 1st Grade Commerce Quiz App

## GitHub Pages पर Deploy कैसे करें (Step by Step)

### Step 1 — GitHub Account बनाएं
1. https://github.com पर जाएं
2. Sign Up करें (Free account)

### Step 2 — New Repository बनाएं
1. GitHub पर Login करें
2. ऊपर **"+"** बटन → **"New repository"** क्लिक करें
3. Repository name: `rpsc-quiz` (या कोई भी नाम)
4. **Public** चुनें
5. **"Create repository"** क्लिक करें

### Step 3 — Files Upload करें
1. Repository खुलने के बाद **"uploading an existing file"** क्लिक करें
2. इन फाइलों को drag-and-drop करें:
   - `index.html`
   - `manifest.json`
   - `sw.js`
3. **"Commit changes"** क्लिक करें

### Step 4 — GitHub Pages Enable करें
1. Repository में **Settings** → **Pages** जाएं
2. Source: **"Deploy from a branch"**
3. Branch: **main** → **/ (root)**
4. **Save** क्लिक करें
5. 2-3 मिनट में आपका URL मिलेगा:
   `https://[आपका-username].github.io/rpsc-quiz/`

---

## App का उपयोग

### JSON Format (प्रश्न कैसे बनाएं)
```json
[
  {
    "q": "यहाँ प्रश्न लिखें",
    "o": ["विकल्प A", "विकल्प B", "विकल्प C", "विकल्प D"],
    "a": 1,
    "exp": "व्याख्या (वैकल्पिक)",
    "src": "RPSC 2022 (वैकल्पिक)",
    "diff": "medium"
  }
]
```
- `a` = सही उत्तर index: 0=A, 1=B, 2=C, 3=D
- `diff` = "easy" / "medium" / "hard"

### प्रश्न अपलोड करने के 3 तरीके:
1. **JSON पेस्ट** — टेक्स्ट कॉपी करके सीधे app में पेस्ट
2. **JSON File** — .json फाइल drag-and-drop
3. **प्रश्न प्रबंधन टैब** — सभी टॉपिक एक जगह देखें

### Data कहाँ सेव होता है?
- सभी प्रश्न आपके **Browser के LocalStorage** में सेव होते हैं
- Internet बंद होने पर भी काम करता है (PWA/Offline)
- **एक device पर अपलोड = उसी device पर दिखेगा**

### सभी devices पर प्रश्न sync करने के लिए:
`questions/` फोल्डर में JSON फाइलें GitHub पर upload करें,
फिर `index.html` में यह code add करें (GitHub से auto-load):

```javascript
// index.html के init() function में जोड़ें:
async function loadFromGitHub() {
  const base = 'https://raw.githubusercontent.com/[username]/rpsc-quiz/main/questions/';
  const topics = ['contract_law', 'partnership', 'company_accounts']; // जो topics load करने हैं
  for(const t of topics) {
    try {
      const r = await fetch(base + t + '.json');
      const data = await r.json();
      if(!QDB[t]) QDB[t] = [];
      QDB[t].push(...data);
    } catch(e) { console.log('Skip:', t); }
  }
  saveQDB();
  updateStats();
}
```

---

## questions/ फोल्डर में JSON फाइलें
हर टॉपिक की एक अलग JSON फाइल बनाएं, नाम = Topic ID:
- `contract_law.json`
- `partnership.json`  
- `company_accounts.json`
- `revolt1857.json`
- आदि...

Topic IDs देखने के लिए: App खोलें → ⚙️ प्रश्न प्रबंधन टैब
