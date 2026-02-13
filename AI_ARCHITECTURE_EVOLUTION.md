# AI Architecture Evolution

วิวัฒนาการของ AI Architecture จากอดีตจนถึงปัจจุบัน

---

## 📜 Timeline

### 2017: Transformer Revolution

**Paper:** "Attention Is All You Need"  
**Authors:** Vaswani et al. (Google)

**ความสำคัญ:**
- เป็นจุดเริ่มต้นของ LLM ยุคใหม่
- แนะนำ Self-Attention Mechanism
- ไม่ต้องใช้ RNN/LSTM แล้ว

**Architecture:**
```
Input → Embedding → [Transformer Block] × N → Output

Transformer Block:
├── Multi-Head Self-Attention
├── Add & Norm
├── Feed-Forward Network
└── Add & Norm
```

---

### 2018-2019: Pre-training Era

**BERT (Google)**
- Bidirectional Encoder
- Masked Language Modeling
- Task-specific fine-tuning

**GPT-1 / GPT-2 (OpenAI)**
- Unidirectional Decoder
- Auto-regressive generation
- Zero-shot learning

---

### 2020: Scale-up

**GPT-3 (OpenAI)**
- 175B parameters
- Few-shot learning
- In-context learning

**Architecture Changes:**
```
Parameters: 117M → 1.5B → 175B
Context: 512 → 1024 → 2048 tokens
Capabilities: ↑↑↑
```

---

### 2022-2023: Instruction Following

**InstructGPT / ChatGPT**
- RLHF (Reinforcement Learning from Human Feedback)
- Instruction tuning
- Better user alignment

**Claude (Anthropic)**
- Constitutional AI
- Harmlessness training
- Safety-first approach

**Architecture:**
```
Pre-training
    ↓
Instruction Tuning
    ↓
RLHF / Constitutional AI
    ↓
Safety Filtering
    ↓
Deployment
```

---

### 2024: Reasoning Models

**o1/o1-preview (OpenAI)**
- Extended thinking time
- Chain-of-Thought natively integrated
- 10-60 seconds reasoning

**Claude Opus/Sonnet 4**
- Visible thinking process
- Structured reasoning
- Safety + reasoning combined

**Architecture:**
```
Input
    ↓
[Reasoning Phase] ← NEW!
│  Step 1: Analyze
│  Step 2: Plan
│  ...
│  Step N: Verify
    ↓
[Generation Phase]
    ↓
Output
```

---

## 🔧 Key Technical Innovations

### 1. Attention Mechanism

**Self-Attention Formula:**
```
Attention(Q, K, V) = softmax(QK^T / √d_k) V

Where:
Q = Query matrix
K = Key matrix
V = Value matrix
d_k = dimension of keys
```

**Why Important:**
- ได้ attention ทุกตำแหน่งในประโยค
- Parallelizable (ไม่ต้อง sequential เหมือน RNN)
- Long-range dependencies

---

### 2. Positional Encoding

**Problem:** Transformer ไม่มี order information

**Solution:**
```python
PE(pos, 2i) = sin(pos / 10000^(2i/d_model))
PE(pos, 2i+1) = cos(pos / 10000^(2i/d_model))
```

**Modern Approach:**
- Rotary Position Embedding (RoPE)
- ALiBi (Attention with Linear Biases)
- Relative positional encoding

---

### 3. Scaling Laws

**Kaplan et al. (2020):**
```
Loss ∝ N^(-α)

Where:
N = number of parameters
α ≈ 0.076
```

**Chinchilla Paper (2022):**
- Optimal compute budget
- Data : Parameters ratio
- "Chinchilla Optimal" models

**Finding:**
```
Better to train:
- Medium-sized model
- Large dataset
- Longer training

Rather than:
- Very large model
- Small dataset
- Short training
```

---

### 4. RLHF (Reinforcement Learning from Human Feedback)

**Process:**
```
1. Pre-trained Model
     ↓
2. Supervised Fine-tuning (SFT)
   - Instruction-response pairs
     ↓
3. Reward Model Training
   - Human preferences
   - Comparison ranking
     ↓
4. PPO Training
   - Optimize for reward
   - KL divergence constraint
     ↓
5. Aligned Model
```

**Components:**
```python
Reward = R(response) - β * KL(π_θ || π_ref)

Where:
R(response) = Reward model score
β = KL penalty coefficient
π_θ = Current policy
π_ref = Reference policy (SFT model)
```

---

### 5. Constitutional AI (Anthropic)

**Principles:**
```yaml
Phase 1: Self-Critique
  - Generate response
  - Critique against principles
  - Revise response

Phase 2: RL from AI Feedback
  - Compare responses
  - Select better response
  - Train on preferences

Principles Examples:
  - "Choose the response that is most helpful and harmless"
  - "Avoid harmful, unethical, or illegal content"
  - "Be honest about uncertainty"
```

---

## 🎯 Current Trends (2024-2026)

### 1. Mixture of Experts (MoE)

**Architecture:**
```
Input
    ↓
Router Network (decides which experts)
    ↓
Expert 1    Expert 2    ...    Expert N
    ↓           ↓               ↓
Combine outputs
    ↓
Output
```

**Benefits:**
- Sparse activation (only some experts active)
- Larger capacity, same compute
- Specialized experts

**Examples:**
- GPT-4 (rumored)
- Mixtral (Mistral AI)

---

### 2. Long Context Windows

**Evolution:**
```
2020: 2K tokens (GPT-3)
2022: 8K tokens (GPT-3.5)
2023: 32K tokens (GPT-4)
2024: 128K tokens (GPT-4 Turbo)
2025: 200K tokens (Claude Sonnet 4)
2026: 1M tokens (Gemini Pro)
```

**Techniques:**
- Sparse attention
- Sliding window attention
- Memory-efficient implementations

---

### 3. Multimodal Models

**Inputs:**
```
Text ──┐
Image ─┼── Unified Encoder ── Transformer ── Output
Video ─┤
Audio ─┘
```

**Examples:**
- GPT-4V (Vision)
- GPT-4o (Omni - all modalities)
- Gemini (native multimodal)

---

### 4. Reasoning Augmentation

**o1-style Reasoning:**
```
Input
    ↓
[Generate reasoning chain]
│  "Let me think step by step..."
│  Step 1: ...
│  Step 2: ...
    ↓
[Verify reasoning]
│  Check consistency
│  Validate logic
    ↓
[Generate final answer]
    ↓
Output
```

**Training:**
- Reward reasoning quality
- Penalize shortcuts
- Teach self-verification

---

## 🔮 Future Directions

### 1. Agent Systems (2025-2027)

**Architecture:**
```
User Request
    ↓
[Planning Agent]
    ↓
Task 1    Task 2    Task 3
  ↓         ↓         ↓
Tool 1    Tool 2    Tool 3
  ↓         ↓         ↓
[Synthesis Agent]
    ↓
Final Result
```

**Capabilities:**
- Multi-step problem solving
- Tool use (API calls, code execution)
- Long-term memory
- Self-improvement

---

### 2. Hybrid Architectures

**Combining:**
```
Fast Path (GPT-4o): Simple queries
    ↓ (if uncertain)
Slow Path (o1): Complex reasoning
```

**Benefits:**
- Best of both worlds
- Cost-effective
- Speed when possible, accuracy when needed

---

### 3. Neurosymbolic AI

**Combining:**
- Neural networks (learning)
- Symbolic reasoning (logic)

**Example:**
```
Neural: "Image contains car"
Symbolic: "Cars have 4 wheels"
Inference: "Image likely shows 4 wheels"
```

---

### 4. Efficient Models

**Goals:**
```
Smaller models with same capabilities
↓
Cheaper inference
↓
Wider deployment
```

**Techniques:**
- Distillation
- Quantization
- Pruning
- Efficient architectures

---

## 📊 Comparison: Then vs Now

| Aspect | 2017 (Original Transformer) | 2024 (Modern LLMs) |
|--------|----------------------------|-------------------|
| **Parameters** | 65M | 70B - 1.8T |
| **Context** | 512 tokens | 200K - 1M tokens |
| **Training Data** | Few GB | 10+ TB |
| **Capabilities** | Translation | General intelligence, reasoning |
| **Training Time** | Days | Months |
| **Training Cost** | $10K | $100M+ |
| **Thinking** | None | Extended reasoning |
| **Safety** | Basic | Constitutional AI, RLHF |

---

## 🎓 Learning Resources

### Papers to Read

1. **Attention Is All You Need** (2017)
   - Original Transformer paper
   - Must-read foundation

2. **BERT: Pre-training of Deep Bidirectional Transformers** (2018)
   - Bidirectional pre-training

3. **Language Models are Few-Shot Learners** (2020)
   - GPT-3 paper
   - In-context learning

4. **Training language models to follow instructions** (2022)
   - InstructGPT paper
   - RLHF explained

5. **Constitutional AI: Harmlessness from AI Feedback** (2022)
   - Claude's approach
   - Safety techniques

6. **Sparks of Artificial General Intelligence** (2023)
   - GPT-4 capabilities
   - AGI discussions

---

### Courses

**Online:**
- Stanford CS224N (NLP with Deep Learning)
- Fast.ai (Practical Deep Learning)
- DeepLearning.AI (Transformer courses)

**Books:**
- "Attention Is All You Need" paper walkthrough
- "The Illustrated Transformer" (Jay Alammar)

---

## 🔄 Changelog

- **2026-02-13:** Initial architecture evolution document
- **Future:** Will update with new developments
