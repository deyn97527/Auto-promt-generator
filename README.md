import streamlit as st
import random
import json

class AIPromptGenerator:
    def __init__(self):
        self.templates = {
            "story": [
                "Write a detailed {genre} story set in {setting}. The main character is {character}, who faces {conflict}.",
                "Create an immersive {genre} narrative about {character} in {setting}, focusing on {theme}."
            ],
            "image": [
                "Generate a highly detailed description of a {style} artwork featuring {subject} in {setting}, with emphasis on {mood} and {lighting}.",
                "Imagine a {style} illustration of {subject} in {setting}, highlighting {details}."
            ],
            "code": [
                "Write optimized {language} code that {functionality}. Include explanations in comments.",
                "Generate a {language} script that {functionality}, ensuring {constraints}."
            ],
            "general": [
                "Explain {topic} in a way that is clear for {audience}, including {extra}.",
                "Provide a comprehensive guide about {topic}, covering {details}."
            ]
        }

        self.options = {
            "genre": ["sci-fi", "fantasy", "mystery", "cyberpunk", "romance"],
            "setting": ["a futuristic city", "a medieval kingdom", "outer space", "an underwater world"],
            "character": ["a rogue hacker", "a young warrior", "an AI companion", "a lost explorer"],
            "conflict": ["a rebellion against tyranny", "a forbidden discovery", "a race against time", "an ancient curse"],
            "theme": ["identity", "power", "freedom", "betrayal"],
            "style": ["cyberpunk", "oil painting", "anime", "surrealist", "steampunk"],
            "subject": ["a futuristic skyline", "a mythical creature", "a robotic samurai", "a haunted forest"],
            "mood": ["dark and mysterious", "bright and hopeful", "chaotic and intense", "dreamlike"],
            "lighting": ["neon-lit", "soft morning light", "dramatic shadows", "golden hour"],
            "details": ["intricate patterns", "realistic textures", "glowing effects", "dynamic perspective"],
            "language": ["Python", "JavaScript", "Rust", "C++"],
            "functionality": ["an AI chatbot", "a web scraper", "a game engine", "a trading bot"],
            "constraints": ["efficiency", "low memory usage", "cross-platform compatibility", "security"],
            "topic": ["quantum computing", "climate change", "blockchain", "psychology of creativity"],
            "audience": ["beginners", "experts", "children", "entrepreneurs"],
            "extra": ["examples", "case studies", "real-world applications", "step-by-step guides"],
            "details": ["advantages and disadvantages", "future trends", "common misconceptions", "practical tips"]
        }

    def generate(self, category="general"):
        if category not in self.templates:
            category = "general"
        template = random.choice(self.templates[category])
        filled_prompt = template
        for key, values in self.options.items():
            if "{" + key + "}" in template:
                filled_prompt = filled_prompt.replace("{" + key + "}", random.choice(values))
        return filled_prompt

# ---------------- STREAMLIT APP ----------------

st.set_page_config(page_title="AI Prompt Generator", page_icon="🤖", layout="centered")
st.title("🤖 AI Auto-Prompt Generator")

st.markdown("Generate creative prompts for **stories, images, code, or general topics**.")

category = st.selectbox("Select a category", ["story", "image", "code", "general"])
generator = AIPromptGenerator()

if st.button("Generate Prompt"):
    prompt = generator.generate(category)
    st.success("Here’s your generated prompt:")
    st.text_area("Prompt", prompt, height=150)

    # Download buttons
    st.download_button(
        label="📥 Download as TXT",
        data=prompt,
        file_name="prompt.txt",
        mime="text/plain"
    )

    st.download_button(
        label="📥 Download as JSON",
        data=json.dumps({"category": category, "prompt": prompt}, indent=2),
        file_name="prompt.json",
        mime="application/json"
    )

st.markdown("---")
st.caption("Built with ❤️ using Streamlit")
