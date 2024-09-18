# Text-to-Speech and Translation in Kurdish (Northern Kurdish, Central Kurdish, and English)

This repository demonstrates the process of synthesizing text-to-speech (TTS) using the [TTS-MMS](https://huggingface.co/facebook/tts_mms) models for **Northern Kurdish (Kurmancî)**, **Central Kurdish (Sorani)**, and **English**, along with text translation using **Google Translator**.

## Features

- **Text-to-Speech (TTS)** for Kurdish and English.
- **Translation** of text between multiple languages (English, Kurdish dialects).
- **Audio saving** in `.wav` format.
- **Number-to-words conversion** for processing numerical data into readable text format in Kurdish.
- **Preprocessing** of text to split into manageable sections for speech synthesis.

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/tts-kurdish.git

2. Install the required packages:
    ```bash
    %pip install ttsmms deep_translator scipy numpy

## Usage
1. Download the TTS models for Northern Kurdish (Kurmancî), Central Kurdish (Sorani), and English using the following code:
    ```python
    from ttsmms import TTS, download

    # Download TTS models
    dir_path_kmr = download("kmr", "./data")  # Northern Kurdish (Kurmancî)
    dir_path_ckb = download("ckb", "./data")  # Central Kurdish (Sorani)
    dir_path_eng = download("eng", "./data")  # English

    # Initialize the TTS models
    tts_kmr = TTS(dir_path_kmr)
    tts_ckb = TTS(dir_path_ckb)
    tts_eng = TTS(dir_path_eng)

2. Translate text between different languages:
    ```python
    from deep_translator import GoogleTranslator

    # Translate English to Central Kurdish (Sorani)
    translator = GoogleTranslator(source='auto', target='ckb')
    translated_text = translator.translate('This is a test.')
    print(translated_text)

3. Synthesize audio from the translated text:
    ```python
    # Synthesize translated text into speech
    wav = tts_ckb.synthesis(translated_text)

    # Play the audio in Jupyter Notebook
    import IPython.display as ipd
    ipd.Audio(wav["x"], rate=wav["sampling_rate"])

    # Save the audio as a .wav file
    import scipy.io.wavfile as wavfile
    output_file = "translated_audio.wav"
    wavfile.write(output_file, wav["sampling_rate"], wav["x"].astype(np.float32))
    print(f"Audio saved successfully to {output_file}")

4. Convert numbers to words for better readability:
    ```python
    def num_to_words(num):
    # Function to convert numbers into words (for Kurdish, add logic)
    ...

    def replace_numbers_with_words(text):
        # Replace all numbers in text with their word equivalents
        ...

    # Example usage
    text_with_numbers = "The year 2020 was tough."
    text_processed = replace_numbers_with_words(text_with_numbers)
    print(text_processed)  # Output: "The year twenty twenty was tough."

5. Batch process multiple text inputs:
    ```python
    text = """ 
    Lorem Ipsum çi ye? 
    Lorem Ipsum bi tenê metnek xapînok a pîşesaziya çapkirin û nivîsandinê ye.
    """

    s_text = text.split('\n')
    preprocess_text = [t for t in s_text if t.strip()]

    for i, text in enumerate(preprocess_text):
        text = replace_numbers_with_words(text)
        wav = tts_kmr.synthesis(text)
        
        # Save each audio file
        output_file = f"output_audio/eng_0{i}.wav"
        wavfile.write(output_file, wav["sampling_rate"], wav["x"].astype(np.float32))
        print(f"Audio saved to {output_file}")


## Dependencies
* ttsmms - TTS model library for multilingual speech synthesis.
* deep_translator - Language translation library.
* scipy - For audio processing and saving.
* numpy - For numerical computations.

## License
This project is licensed under the MIT License - see the LICENSE file for details.
