# Art Bell Transcripts

This repository contains cleaned transcripts of [Art Bell](https://en.wikipedia.org/wiki/Art_Bell)'s radio broadcasts, covering 1530 Art-hosted episodes of [Coast to Coast AM](https://en.wikipedia.org/wiki/Coast_to_Coast_AM), Area 2000, [Dreamland](https://en.wikipedia.org/wiki/Coast_to_Coast_AM#Dreamland), [Dark Matter](https://en.wikipedia.org/wiki/Art_Bell%27s_Dark_Matter), and [Midnight in the Desert](https://en.wikipedia.org/wiki/Art_Bell#Midnight_in_the_Desert_radio_show).

## Motivation

This archive was created because complete and accurate text transcriptions of these unique and culturally significant broadcasts contributes to the historical record in a more easily-processed way than existing audio archives.

## Process

### Preparation
1.  Create a vocabulary of episode-specific names, terminology, and historical events.
2.  Create voice signatures for recurring speakers (e.g. Art Bell, [Ross Mitchell](https://en.wikipedia.org/wiki/Ross_Mitchell), [Terence McKenna](https://en.wikipedia.org/wiki/Terence_McKenna), [John Lear](https://en.wikipedia.org/wiki/John_Lear)) from representative clips covering the spectrum of programs, years, and recording quality.
3.  Enroll recurring speakers using [pyannote Speaker Diarization Community-1](https://huggingface.co/pyannote/speaker-diarization-community-1). 

### Transcription
4. Decode episode MP3 with FFmpeg to 16kHz mono.
5. Transcribe with [Qwen3-ASR 1.7B](https://huggingface.co/Qwen/Qwen3-ASR-1.7B) in BF16 through vLLM, using 45-second windows, one second of context on each side, batches of 32 and the episode vocabulary.
6. Assign word timestamps with [Qwen3-ForcedAligner 0.6B](https://huggingface.co/Qwen/Qwen3-ForcedAligner-0.6B). Use those timestamps to remove duplicated text at window boundaries while preserving spoken repetitions.
7. Identify speakers with [pyannote Community-1](https://huggingface.co/pyannote/speaker-diarization-community-1) over the full recording. Cross-check speaker activity with [Nemotron-3-Diarization](https://huggingface.co/nvidia/Nemotron-3-Diarization) in five-minute blocks. Assign names using voice signatures, introductions and caller handoffs. Number unnamed voices and callers.
8. Recheck suspected omissions, interruptions and overlaps with [MOSS-Transcribe-Diarize 0.9B](https://huggingface.co/OpenMOSS-Team/MOSS-Transcribe-Diarize) in BF16 and [Whisper large-v3](https://huggingface.co/Systran/faster-whisper-large-v3) in FP16 through [faster-whisper](https://github.com/SYSTRAN/faster-whisper). Use [Microsoft VibeVoice-ASR-HF 8B](https://huggingface.co/microsoft/VibeVoice-ASR-HF) for disputed speakers and [TorchAudio WAV2VEC2_ASR_BASE_960H](https://docs.pytorch.org/audio/stable/generated/torchaudio.pipelines.WAV2VEC2_ASR_BASE_960H.html) for disputed word boundaries.
9. Check for missing words with a second full-recording pass of [Qwen3-ASR-1.7B](https://huggingface.co/Qwen/Qwen3-ASR-1.7B), using 100-second windows without vocabulary hints. Verify proposed additions with [MOSS-Transcribe-Diarize 0.9B](https://huggingface.co/OpenMOSS-Team/MOSS-Transcribe-Diarize), [Whisper large-v3](https://huggingface.co/Systran/faster-whisper-large-v3), [wav2vec2 alignment](https://docs.pytorch.org/audio/stable/generated/torchaudio.pipelines.WAV2VEC2_ASR_BASE_960H.html) and [IBM Granite Speech 5.0 470M TurboCTC](https://huggingface.co/ibm-granite/granite-speech-5.0-470m-turboctc). Recheck disputed speaker assignments with [MOSS-Transcribe-Diarize 0.9B](https://huggingface.co/OpenMOSS-Team/MOSS-Transcribe-Diarize).
10. Check for missing speaker turns with [Granite Speech 5.0 470M TurboCTC](https://huggingface.co/ibm-granite/granite-speech-5.0-470m-turboctc) over the full recording in 55-second windows. Verify candidates with the [MOSS](https://huggingface.co/OpenMOSS-Team/MOSS-Transcribe-Diarize), [Whisper](https://huggingface.co/Systran/faster-whisper-large-v3) and [alignment](https://docs.pytorch.org/audio/stable/generated/torchaudio.pipelines.WAV2VEC2_ASR_BASE_960H.html) models. Use [NVIDIA Nemotron-3.5-Transcribe-30B-A4B](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B) when a turn needs another independent wording check.
11. Flag possible commercial breaks, bumper music and transitions by checking the first and last 90 seconds and cues such as “we’ll be right back,” station IDs and “welcome back.” Record candidate times for review and preserve spoken ads, announcements and speech over music.
12. Group speech into turns.
    
### LLM-guided Cleanup
LLM-guided cleanup
13. Load the latest accumulated knowledge guide and cleanup rules.
14. Establish the original broadcast date, program, host, guests and topics. 
15. Verify names, places, books, organisations and technical terms. Preserve spoken factual mistakes. Correct spellings, acronyms, call signs and program terminology in accordance with the broadcast period.
16. Check for short interjections and assign the interruption to its speaker. 
17. Number callers and unknown speakers by their first appearance. Add a clearly introduced personal name or spoken on-air nickname. Use established names for identifiable community figures.
18. Identify bumper music lyrics around ad-breaks by comparing against the accumulated bumper music catalogue.
19. Remove unrelated generic advertisements and non-program material. Retain community-related ads.
20. Remove archive branding, Somewhere in Time references, and replay warnings. 
21. Restore missing intro or signoff wording supported by any surviving fragments and knowledge of the program's era. 
22. Remove obvious stutters and duplicated low-information prefixes. Preserve emphasis, grammatical repetition, self-corrections, and questions repeated because a connection failed.
23. Format years, decades, telephone numbers, station frequencies, quantities and currency consistently. 
24. Repair punctuation around questions, quotations, cutoffs and interruptions.
25. Update accumulated knowledge guide and cleanup rules.
