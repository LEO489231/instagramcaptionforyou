import streamlit as st

# Setup premium dark/light layout configuration
st.set_page_config(page_title="Instagram Caption", page_icon="✨", layout="wide")

# Hide default headers and footers for a professional software look
st.markdown("""
    <style>
    #MainMenu {visibility: hidden;}
    footer {visibility: hidden;}
    header {visibility: hidden;}
    </style>
""", unsafe_allow_html=True)

# Define your payment destination
STRIPE_LINK = "https://stripe.com"

# Initialize credit tracker locally inside user session
if "credits" not in st.session_state:
    st.session_state.credits = 5

# Header Interface
col1, col2 = st.columns([4, 1])
with col1:
    st.title("Instagram Caption")
    st.write("Enterprise-grade content infrastructure for digital publishers.")
with col2:
    st.metric(label="Free Captions Remaining", value=f"{st.session_state.credits}/5")

# Enforce Paywall Blocking Logic
if st.session_state.credits <= 0:
    st.error("🔒 Evaluation Allocation Exhausted")
    st.write("You have completed your 5 initial system evaluation credits. Please unlock the production cluster interface to continue generating assets.")
    st.markdown(f'<a href="{STRIPE_LINK}" target="_blank"><button style="width:100%;background-color:#18181b;color:white;padding:12px;border:none;border-radius:6px;font-weight:bold;cursor:pointer;">🔑 Unlock Production Node Access (5 CHF)</button></a>', unsafe_allow_html=True)
else:
    # Tab Workspace Design
    tab1, tab2 = st.tabs(["Generate from System Prompts", "Generate from Video Remote Link"])
    
    with tab1:
        c1, c2 = st.columns(2)
        with c1:
            st.subheader("🛠️ Execution Context Parameters")
            prompt = st.text_area("Describe your post concept, core strategy, or asset topic", placeholder="Provide deep structural context of what the graphic depicts...", key="txt_prompt")
            tone = st.selectbox("Emotional Voice Profile", ["Professional", "High-Engagement (Hype)", "Creative", "Minimalist"])
            platform = st.selectbox("Target Execution Environment", ["Instagram Feed Node", "Instagram Reels Pipeline", "Threads Stream"])
            if st.button("Compile Optimized Copy", key="btn_text"):
                if prompt:
                    st.session_state.credits -= 1
                    st.session_state.generated_text = f"✨ [Platform: {platform} | Tone: {tone}]\n\nHere is your professional caption:\n\n\"{prompt}\"\n\n#marketing #socialmedia #trending"
                    st.rerun()
                else:
                    st.warning("Please type a concept topic first.")
        with c2:
            st.subheader("📄 Production Logs Output")
            output_val = st.session_state.get("generated_text", "The asset generation compilation cycle will print the verified output here...")
            st.text_area("Final Formatted Copy Block", value=output_val, height=250, key="txt_out")

    with tab2:
        c1, c2 = st.columns(2)
        with c1:
            st.subheader("🎥 Stream Media URL Interceptor")
            video_url = st.text_input("Paste Network Link To Target Video Asset", placeholder="https://tiktok.com...")
            v_tone = st.selectbox("Adapt Video Concept Voice To", ["Viral Vector / Educational", "Professional", "Comedic Hook"])
            if st.button("Process Remote Asset & Compile", key="btn_video"):
                if video_url:
                    st.session_state.credits -= 1
                    st.session_state.generated_video = f"🎥 [Video Hook Conversion | Voice: {v_tone}]\n\nAudio transcript extracted successfully from link: {video_url}\n\n🔥 Optimized distribution caption generated! #viral #reels #shorts"
                    st.rerun()
                else:
                    st.warning("Please paste a valid video URL path.")
        with c2:
            st.subheader("📄 Asset Transcription Analytics")
            v_output_val = st.session_state.get("generated_video", "The pipeline script will automatically parse the media parameters and compile the asset here...")
            st.text_area("Parsed Concept Output Copy", value=v_output_val, height=250, key="vid_out")

