# Art Bell Transcripts

This repository contains cleaned transcripts of Art Bell's radio broadcasts, covering 1530 Art-hosted episodes of Coast to Coast AM, Area 2000, Dreamland, Dark Matter, and Midnight in the Desert.

## Motivation

This archive was created because complete and accurate text transcriptions of these unique and culturally significant broadcasts contributes to the historical record in a more easily-processed way than existing audio archives.

## Process

### Preparation
1.  Create a vocabulary of episode-specific names, terminology, and historical events.
2.  Create voice signatures for recurring speakers (e.g. Art Bell, Ross Mitchell, Terence McKenna, John Lear) from representative clips covering the spectrum of programs, years, and recording quality.
3.  Enroll recurring speakers using pyannote Speaker Diarization Community-1. 

### Transcription
4. Decode episode MP3 with FFmpeg to 16kHz mono.
5. Transcribe with Qwen3-ASR 1.7B in BF16 through vLLM, using 45-second windows, one second of context on each side, batches of 32 and the episode vocabulary.
6. Assign word timestamps with Qwen3-ForcedAligner 0.6B. Use those timestamps to remove duplicated text at window boundaries while preserving spoken repetitions.
7. Identify speakers with pyannote Community-1 over the full recording. Cross-check speaker activity with Nemotron-3-Diarization in five-minute blocks. Assign names using voice signatures, introductions and caller handoffs. Number unnamed voices and callers.
8. Recheck suspected omissions, interruptions and overlaps with MOSS-Transcribe-Diarize 0.9B in BF16 and Whisper large-v3 in FP16 through faster-whisper. Use Microsoft VibeVoice-ASR-HF 8B for disputed speakers and TorchAudio WAV2VEC2_ASR_BASE_960H for disputed word boundaries.
9. Check for missing words with a second full-recording pass of Qwen3-ASR-1.7B, using 100-second windows without vocabulary hints. Verify proposed additions with MOSS-Transcribe-Diarize 0.9B, Whisper large-v3, wav2vec2 alignment and IBM Granite Speech 5.0 470M TurboCTC. Recheck disputed speaker assignments with MOSS-Transcribe-Diarize 0.9B.
10. Check for missing speaker turns with Granite Speech 5.0 470M TurboCTC over the full recording in 55-second windows. Verify candidates with the MOSS, Whisper and alignment models from step 7. Use NVIDIA Nemotron-3.5-Transcribe-30B-A4B when a turn needs another independent wording check.
11. Flag possible commercial breaks, bumper music and transitions by checking the first and last 90 seconds and cues such as “we’ll be right back,” station IDs and “welcome back.” Record candidate times for review and preserve spoken ads, announcements and speech over music.
12. Group speech into turns.
    
### LLM-guided Cleanup
13. 
