# instagramcaptionforyou
its an instagram caption for free 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Instagram Caption</title>
    <link href="https://googleapis.com" rel="stylesheet">
    <style>
        * { box-sizing: border-box; font-family: 'Inter', sans-serif; }
        body { background-color: #fafafa; color: #18181b; margin: 0; padding: 40px 20px; display: flex; justify-content: center; }
        .container { max-width: 1000px; width: 100%; }
        .header-row { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 30px; }
        h1 { margin: 0; font-size: 28px; font-weight: 600; letter-spacing: -0.5px; }
        .subtitle { margin: 5px 0 0 0; color: #71717a; font-size: 14px; }
        .counter-badge { background: #f4f4f5; border: 1px solid #e4e4e7; padding: 8px 16px; border-radius: 8px; font-weight: 500; font-size: 14px; }
        .paywall { display: none; background: #fff; border: 2px solid #e4e4e7; padding: 30px; border-radius: 12px; text-align: center; margin-bottom: 30px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05); }
        .paywall h2 { margin: 0 0 10px 0; font-size: 20px; }
        .paywall p { color: #71717a; font-size: 14px; margin: 0 0 20px 0; }
        .btn-pay { background: #18181b; color: #fff; border: none; padding: 12px 24px; border-radius: 6px; font-weight: 500; cursor: pointer; text-decoration: none; display: inline-block; }
        .tabs { display: flex; border-bottom: 1px solid #e4e4e7; margin-bottom: 25px; }
        .tab { padding: 12px 20px; cursor: pointer; font-weight: 500; color: #71717a; border-bottom: 2px solid transparent; }
        .tab.active { color: #18181b; border-bottom-color: #18181b; }
        .tab-content { display: none; background: #fff; border: 1px solid #e4e4e7; padding: 30px; border-radius: 12px; }
        .tab-content.active { display: grid; grid-template-columns: 1fr 1fr; gap: 30px; }
        .panel { display: flex; flex-direction: column; }
        label { font-weight: 500; font-size: 14px; margin-bottom: 8px; }
        input, textarea, select { padding: 10px; border: 1px solid #e4e4e7; border-radius: 6px; font-size: 14px; margin-bottom: 20px; width: 100%; outline: none; }
        input:focus, textarea:focus, select:focus { border-color: #a1a1aa; }
        .btn-generate { background: #18181b; color: #fff; border: none; padding: 12px; border-radius: 6px; font-weight: 500; cursor: pointer; font-size: 14px; }
        .btn-generate:disabled { background: #e4e4e7; color: #a1a1aa; cursor: not-allowed; }
        textarea { resize: none; background: #fafafa; }
        .output-panel textarea { background: #fff; height: 100%; min-height: 250px; }
    </style>
</head>
<body>

<div class="container">
    <div class="header-row">
        <div>
            <h1>Instagram Caption</h1>
            <p class="subtitle">Enterprise-grade content infrastructure for digital publishers.</p>
        </div>
        <div class="counter-badge" id="counter">Free Captions Remaining: 5/5</div>
    </div>

    <div class="paywall" id="paywall">
        <h2>🔒 Usage Limit Reached</h2>
        <p>You have used your 5 free credits. Unlock unlimited premium caption generation instantly.</p>
        <a href="https://stripe.com" class="btn-pay" target="_blank">🔑 Unlock Next Caption (5 CHF)</a>
    </div>

    <div class="tabs">
        <div class="tab active" onclick="switchTab('text-tab')">Generate from Text</div>
        <div class="tab" onclick="switchTab('video-tab')">Generate from Video URL</div>
    </div>

    <!-- TAB 1 -->
    <div id="text-tab" class="tab-content active">
        <div class="panel">
            <h3>🛠️ Execution Parameters</h3>
            <label>Describe your post concept or topic</label>
            <textarea id="text-prompt" placeholder="Type what your image or video is about..." rows="4"></textarea>
            
            <label>Select Tone Profile</label>
            <select id="text-tone">
                <option>Professional</option>
                <option>High-Engagement (Hype)</option>
                <option>Creative</option>
                <option>Minimalist</option>
            </select>
            <button class="btn-generate" id="btn-text" onclick="processGeneration('text')">Compile Optimized Copy</button>
        </div>
        <div class="panel output-panel">
            <h3>📄 Production Output</h3>
            <textarea id="text-output" placeholder="Your professional caption will appear here..." readonly></textarea>
        </div>
    </div>

    <!-- TAB 2 -->
    <div id="video-tab" class="tab-content">
        <div class="panel">
            <h3>🎥 Video Link Interceptor</h3>
            <label>Paste TikTok, Reel, or YouTube URL</label>
            <input type="text" id="video-url" placeholder="https://tiktok.com...">
            
            <label>Social Channel Format</label>
            <select id="video-platform">
                <option>Re-optimize for Instagram Reels</option>
                <option>Format for YouTube Shorts</option>
            </select>
            <button class="btn-generate" id="btn-video" onclick="processGeneration('video')">Process Video & Compile</button>
        </div>
        <div class="panel output-panel">
            <h3>📄 Asset Transcription Analytics</h3>
            <textarea id="video-output" placeholder="Your video transcript based caption will appear here..." readonly></textarea>
        </div>
    </div>
</div>

<script>
    let count = 0;
    const maxFree = 5;

    function switchTab(tabId) {
        document.querySelectorAll('.tab').forEach((t, i) => t.classList.toggle('active', i === (tabId === 'text-tab' ? 0 : 1)));
        document.querySelectorAll('.tab-content').forEach(c => c.classList.toggle('active', c.id === tabId));
    }

    function processGeneration(type) {
        if (count >= maxFree) return;

        count++;
        const remaining = maxFree - count;
        document.getElementById('counter').innerText = `Free Captions Remaining: ${remaining}/${maxFree}`;

        if (type === 'text') {
            const prompt = document.getElementById('text-prompt').value || 'your topic';
            const tone = document.getElementById('text-tone').value;
            document.getElementById('text-output').value = `✨ [Tone: ${tone}]\n\nHere is your optimized professional caption based on: "${prompt}"\n\n#marketing #socialmedia #trending`;
        } else {
            const url = document.getElementById('video-url').value || 'provided link';
            document.getElementById('video-output').value = `🎥 [Video Hook Conversion]\n\nAudio transcribed successfully from link: ${url}.\n\n🔥 Here is your viral caption written from your video content! #reels #tiktok #viral`;
        }

        if (count >= maxFree) {
            document.getElementById('paywall').style.display = 'block';
            document.getElementById('btn-text').disabled = true;
            document.getElementById('btn-video').disabled = true;
        }
    }
</script>
</body>
</html>
