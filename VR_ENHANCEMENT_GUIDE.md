# PhrenTech VR Enhancement Guide

## Making Your VR Environments Truly Realistic

### Option 1: Use Real 360° Photos (EASIEST & BEST)

#### Free 360° Photo Resources:
1. **Unsplash** - https://unsplash.com/s/photos/360
   - Search: "360 forest", "360 beach", "360 zen garden"
   - High quality, free to use
   - Download as JPG

2. **Pexels** - https://www.pexels.com/search/360/
   - Free stock 360° photos
   - No attribution required

3. **Flickr** - Search "equirectangular" or "360 photo"
   - Filter by Creative Commons license
   - Massive collection

#### How to Add to Your Code:
In your HTML file, find the `environments` object and add `imageUrl`:

```javascript
zen: {
    imageUrl: 'https://images.unsplash.com/photo-1234567890',  // Add this line
    name: 'Zen Garden',
    // ... rest stays the same
}
```

#### Best Image Specs:
- **Format**: JPG or PNG
- **Aspect Ratio**: 2:1 (equirectangular)
- **Resolution**: 4096x2048 or higher
- **File Size**: Under 5MB for web

---

### Option 2: Create Your Own 360° Content

#### Recommended 360° Cameras:
1. **Insta360 X3** ($450) - Best quality
2. **Ricoh Theta Z1** ($1000) - Professional
3. **GoPro Max** ($500) - Good all-around
4. **Insta360 ONE X2** ($300) - Budget-friendly

#### Shoot Your Own Therapy Spaces:
1. Find serene locations (parks, beaches, gardens)
2. Mount camera on tripod at eye level (~5.5 feet)
3. Use timer or remote to avoid being in shot
4. Shoot in good lighting (golden hour is best)
5. Export as equirectangular JPG

---

### Option 3: Use AI to Generate 360° Images

#### AI Tools for 360° Generation:
1. **Skybox AI** - https://skybox.blockadelabs.com/
   - Type: "Peaceful zen garden at sunset"
   - Generates perfect 360° images
   - Free tier available

2. **Stable Diffusion 360°**
   - Use "equirectangular" in your prompt
   - Requires technical setup

---

## Getting Natural-Sounding AI Voices

### The Problem:
Web Speech API (currently used) sounds robotic because it uses your device's built-in voices.

### Solution: Premium Text-to-Speech APIs

#### Best Options:

### 1. **ElevenLabs** (MOST NATURAL)
- **Cost**: $5/month for 30,000 characters
- **Quality**: Indistinguishable from human
- **Setup**: 10 minutes
- **Website**: https://elevenlabs.io

**How to Integrate:**
```javascript
async function speak(text) {
    const response = await fetch('https://api.elevenlabs.io/v1/text-to-speech/VOICE_ID', {
        method: 'POST',
        headers: {
            'xi-api-key': 'YOUR_API_KEY',
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            text: text,
            voice_settings: {
                stability: 0.8,
                similarity_boost: 0.8
            }
        })
    });
    const audioBlob = await response.blob();
    const audio = new Audio(URL.createObjectURL(audioBlob));
    audio.play();
}
```

**Recommended Voices:**
- "Rachel" - Warm, professional therapist
- "Bella" - Calm, soothing
- "Elli" - Gentle, empathetic

---

### 2. **Google Cloud Text-to-Speech**
- **Cost**: Free for first 1 million characters/month
- **Quality**: Very natural (WaveNet voices)
- **Setup**: 20 minutes
- **Website**: https://cloud.google.com/text-to-speech

**Best Voices for Therapy:**
- `en-US-Neural2-F` - Female, professional
- `en-US-Wavenet-H` - Female, calming
- `en-US-Neural2-A` - Female, warm

---

### 3. **Amazon Polly (Neural)**
- **Cost**: $16 per 1 million characters
- **Quality**: Natural, but slightly less than ElevenLabs
- **Setup**: 15 minutes

**Best Voice:**
- `Joanna (Neural)` - Perfect for therapeutic applications

---

### 4. **Azure Speech Services**
- **Cost**: Free tier: 0.5 million characters/month
- **Quality**: Very good
- **Setup**: 15 minutes

**Best Voices:**
- `en-US-JennyNeural` - Friendly, warm
- `en-US-AriaNeural` - Professional, clear

---

## Implementation Priority

### Quick Wins (Do These First):

1. **Add Real 360° Photos** (30 minutes)
   - Find 3 good photos on Unsplash
   - Add `imageUrl` to each environment
   - Test immediately

2. **Sign up for ElevenLabs** (1 hour)
   - Create account
   - Get API key
   - Replace speech function
   - Huge quality improvement

### Advanced (For Production):

3. **Shoot Custom 360° Content** (1-2 days)
   - Rent/buy 360° camera
   - Visit real therapeutic spaces
   - Capture your actual therapy rooms
   - Professional quality

4. **Add More Therapist Features**
   - Lip sync with audio
   - Realistic facial expressions
   - Hand gestures that match dialogue
   - Multiple therapist avatars

---

## Code Integration Example

### Complete ElevenLabs Integration:

```javascript
// Add this at the top of your script
const ELEVENLABS_API_KEY = 'your_api_key_here';
const VOICE_ID = 'EXAVITQu4vr4xnSDxMaL'; // Rachel voice

async function speakWithElevenLabs(text) {
    try {
        const response = await fetch(`https://api.elevenlabs.io/v1/text-to-speech/${VOICE_ID}`, {
            method: 'POST',
            headers: {
                'Accept': 'audio/mpeg',
                'xi-api-key': ELEVENLABS_API_KEY,
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                text: text,
                model_id: 'eleven_monolingual_v1',
                voice_settings: {
                    stability: 0.75,
                    similarity_boost: 0.85,
                    style: 0.5,
                    use_speaker_boost: true
                }
            })
        });

        if (!response.ok) {
            throw new Error('API request failed');
        }

        const audioBlob = await response.blob();
        const audioUrl = URL.createObjectURL(audioBlob);
        const audio = new Audio(audioUrl);
        
        audio.onplay = () => animateTherapistSpeaking(true);
        audio.onended = () => {
            animateTherapistSpeaking(false);
            URL.revokeObjectURL(audioUrl);
            // Continue to next dialogue
            currentDialogueIndex++;
            setTimeout(() => {
                if (sessionActive && !sessionPaused) {
                    speakDialogue();
                }
            }, 3500);
        };
        
        audio.play();
        
    } catch (error) {
        console.error('Speech error:', error);
        // Fallback to Web Speech API
        speakWithWebAPI(text);
    }
}
```

---

## Free 360° Photo Recommendations by Environment

### Zen Garden:
- Search: "Japanese garden 360" or "zen temple 360"
- Look for: Stone gardens, bamboo, minimalist spaces
- Example: https://unsplash.com/s/photos/zen-garden-360

### Beach:
- Search: "beach sunset 360" or "ocean panorama"
- Look for: Golden hour, calm waters, minimal people
- Example: https://unsplash.com/s/photos/beach-sunset-360

### Forest:
- Search: "forest path 360" or "woods panorama"  
- Look for: Dense trees, natural light filtering through
- Example: https://unsplash.com/s/photos/forest-360

---

## Budget Breakdown

### Minimal Budget ($5/month):
- ElevenLabs voice: $5/month
- Free 360° photos from Unsplash: $0
- **Total**: $5/month for professional quality

### Professional Budget ($500 one-time + $5/month):
- Insta360 X3 camera: $450
- ElevenLabs voice: $5/month  
- Tripod: $30
- Editing software: $20
- **Total**: $500 initial + $5/month

### Enterprise Budget ($2000-5000):
- Professional 360° camera: $1000
- Multiple location shoots: $500-2000
- Professional voice actor recording: $500-1000
- Custom 3D therapist model: $500-1000
- **Total**: $2500-5000 one-time

---

## Testing Your Changes

### After Adding 360° Photos:
1. Open the page
2. Should see realistic environments immediately
3. Drag to look around
4. Check on mobile with gyroscope

### After Adding ElevenLabs:
1. Click "Start Guided Session"
2. Voice should sound like a real person
3. Check natural pauses and intonation
4. Verify it syncs with avatar animations

---

## Next Steps

1. **This Week**: Add 360° photos (30 min)
2. **Next Week**: Set up ElevenLabs (1 hour)
3. **This Month**: Consider shooting custom content
4. **Long Term**: Build full production VR app with Unity/Unreal

---

## Questions?

Common issues and solutions:

**Q: 360° image looks stretched**
A: Make sure it's 2:1 aspect ratio (equirectangular format)

**Q: Avatar still not visible**
A: Try looking straight ahead and slightly down when page loads

**Q: Voice still sounds robotic**
A: Web Speech API has limitations. You MUST use ElevenLabs or similar for natural voice

**Q: Images load slowly**
A: Compress to under 2MB using tools like TinyPNG

---

## Resources

- Three.js Docs: https://threejs.org/docs/
- WebXR Docs: https://immersiveweb.dev/
- 360° Photo Tips: https://www.insta360.com/blog/
- ElevenLabs API: https://docs.elevenlabs.io/

---

Good luck with your PhrenTech platform! The combination of real 360° photos and ElevenLabs voices will make a HUGE difference in realism and investor appeal.
