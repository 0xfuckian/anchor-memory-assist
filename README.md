# anchor-memory-assist
# Anchor

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue.svg)
![Status: prototype](https://img.shields.io/badge/status-prototype-orange.svg)
[![GitHub stars](https://img.shields.io/github/stars/0xfuckian/anchor?style=social)](https://github.com/YOUR-USERNAME/anchor)

An open-source wearable memory assistant for people with Alzheimer's and other dementias. Anchor listens for what someone is doing (or has just been asked to do) and gently reminds them, out loud, when they lose track.

> **Status: early prototype.** Being built and tested first for one family member. Expect rough edges.

## The problem

People with memory loss often forget what they were in the middle of doing: why they walked into the kitchen, what they were about to take, where they were going. Most dementia tech focuses on GPS tracking and medication dispensers. Very little helps in the moment with "what was I doing?"

## How Anchor helps through the day

Anchor is meant to fill the small gaps that make a day hard for someone with memory loss, without taking over. These are the situations it's being designed around. None of this is proven yet; part of the project is finding out what actually helps.

**Morning routine.** Someone says "time to get dressed and have breakfast." Anchor notes the task. If the person stands in the hallway unsure why they're there, it says, "You're getting dressed, then having breakfast."

**Losing the thread mid-task.** The person walks into the kitchen and says, "What was I doing?" Anchor answers from its running log: "You came in to take your pills."

**Being given a task by someone else.** A family member says, "Can you grab the mail?" If the person drifts off, Anchor repeats the task calmly, then stops as soon as they get back on track.

**Fewer questions for caregivers.** The same question asked over and over ("What am I doing? Where am I going?") can be answered by the device, which takes some of that load off family and caregivers.

**Independence and dignity.** Reminders are short, calm, and use the person's name. The goal is to help them finish what they started themselves instead of being told what to do by someone else.

**Designed to back off.** Reminders repeat on a set interval, then stop after the person confirms or after a few tries, so the device doesn't become a source of agitation.

**Every person is different.** Interval, wording, and quiet hours are configurable, since what helps one person can upset another.

## How it works


1. An [Omi](https://github.com/BasedHardware/omi) pendant streams live audio to the Omi phone app.
2. The Omi app sends real-time transcripts to Anchor's server through a webhook.
3. Anchor keeps a short running log of the person's current task ("getting pills in the kitchen").
4. When it detects that the person forgot or lost track, it repeats a short, calm reminder on a configurable interval (default: every 15 seconds) until they confirm or move on.

```
Omi pendant -> Omi app -> Anchor server (webhook) -> reminder -> phone / earbuds
```

## Features (planned)

- [ ] Webhook receiver for Omi real-time transcripts
- [ ] Current-task tracking
- [ ] Trigger detection: task given, "what was I doing?", "I forgot"
- [ ] Reminder loop with configurable interval and automatic back-off
- [ ] Per-person settings: name, wording style, quiet hours
- [ ] Spoken output (text-to-speech)
- [ ] Caregiver alert if the device goes offline
- [ ] Optional camera input (Omi glass or a DIY camera)

## Getting started

Requirements: Python 3.10+, an Omi device and the Omi app, and an LLM API key.

```bash
git clone https://github.com/YOUR-USERNAME/anchor.git
cd anchor
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env    # add your API keys
uvicorn server.main:app --reload
```

Expose your local server with a tunnel (ngrok or Cloudflare Tunnel), then in the Omi app go to **Explore > Create an App**, choose the real-time transcript capability, and paste your webhook URL.

## Configuration

Copy `config.example.yaml` and adjust:

- `interval_seconds`: how often to repeat a reminder
- `max_repeats`: how many times before backing off
- `person_name`: how reminders address the person
- `quiet_hours`: when reminders are silenced

The right settings differ a lot from person to person. Talk to the person's doctor or an occupational therapist about what cadence and wording suit them.

## Privacy and consent

Anchor processes continuous audio. If you use it:

- Get consent from the person wearing it, or from whoever holds their legal authority to decide.
- Tell anyone else who may be recorded around them.
- Keep transcripts and API keys under your own control. Never commit `.env`.

## Important

Anchor is not a medical device and does not diagnose, treat, or monitor any condition. It is a reminder aid and can fail (dropped Bluetooth, dead battery, bad transcription). Don't rely on it for safety-critical needs like medication without other safeguards.

## Contributing

Caregivers, clinicians, and developers are welcome. Open an issue describing what worked or didn't for the person you're helping.

## License

MIT

