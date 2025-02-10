# YouTube Video Summarizer

## Overview
The YouTube Video Summarizer is a web application that extracts transcripts from YouTube videos and generates concise summaries using the Gemini Pro API. This tool helps users quickly grasp key insights from lengthy videos by providing detailed notes in bullet points.

## Features
- Extracts transcript from any YouTube video
- Generates a structured summary using Google's Gemini Pro API
- Provides a clean and user-friendly interface using Streamlit
- Displays the video's thumbnail for better identification

## Components Used
- **Streamlit**: For creating the web-based user interface
- **Google Gemini Pro API**: For generating AI-powered summaries
- **YouTube Transcript API**: For retrieving video transcripts
- **Dotenv**: For managing API keys securely

## Installation and Setup
### Prerequisites
Ensure you have Python installed on your system. Install required dependencies using:
```bash
pip install streamlit google-generativeai python-dotenv youtube-transcript-api
```

### Setting Up API Keys
1. Obtain your Google Gemini Pro API key from [Google AI Studio](https://aistudio.google.com/).
2. Create a `.env` file in the project directory and add your API key:
   ```env
   GOOGLE_API_KEY=your_api_key_here
   ```

### Running the Application
Execute the following command to launch the Streamlit app:
```bash
streamlit run app.py
```

## Usage
1. Enter a YouTube video link in the input box.
2. Click the "Get Detailed Notes" button.
3. View the extracted transcript summary.
4. The application will display the video thumbnail along with the AI-generated summary.

## Code Explanation
### Extracting YouTube Video Transcripts
```python
def extract_transcript_details(youtube_video_url):
    try:
        video_id = youtube_video_url.split("=")[1]
        transcript_text = YouTubeTranscriptApi.get_transcript(video_id)
        transcript = " ".join([i["text"] for i in transcript_text])
        return transcript
    except Exception as e:
        raise e
```
This function extracts the transcript from a YouTube video using the YouTube Transcript API.

### Generating Summaries with Gemini Pro
```python
def generate_gemini_content(transcript_text, prompt):
    model = genai.GenerativeModel("gemini-pro")
    response = model.generate_content(prompt + transcript_text)
    return response.text
```
This function sends the transcript to the Gemini Pro API along with a predefined prompt to generate a structured summary.

### Streamlit Interface
```python
st.title("YouTube Transcript to Detailed Notes Converter")
youtube_link = st.text_input("Enter YouTube Video Link:")
if youtube_link:
    video_id = youtube_link.split("=")[1]
    st.image(f"http://img.youtube.com/vi/{video_id}/0.jpg", use_column_width=True)
if st.button("Get Detailed Notes"):
    transcript_text = extract_transcript_details(youtube_link)
    if transcript_text:
        summary = generate_gemini_content(transcript_text, prompt)
        st.markdown("## Detailed Notes:")
        st.write(summary)
```
This part of the code creates a simple Streamlit interface where users can enter a YouTube link and receive a summarized transcript.

## Potential Improvements
- **Enhance error handling**: Improve handling of invalid or unavailable transcripts.
- **Support multiple languages**: Extend the summarization capability to multilingual transcripts.
- **Customize summary length**: Allow users to select the desired length of the summary.
- **Downloadable summaries**: Provide an option to download the summary as a text file.


## Contributing
Contributions are welcome! Feel free to fork the repository and submit pull requests with improvements.

## Contact
For any issues or feature requests, please reach out via GitHub.

---
