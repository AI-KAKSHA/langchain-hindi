# LangGraph Framework

LangGraph एक फ्रेमवर्क है जो **LangChain** का विस्तार (extension) है। यह **ग्राफ-आधारित आर्किटेक्चर** पर काम करता है, जहाँ वर्कफ्लो को नोड्स और एजेस के रूप में प्रस्तुत किया जाता है। LangGraph का उद्देश्य एआई वर्कफ्लो को और अधिक मॉड्यूलर और विज़ुअलाइज़ेबल बनाना है, जिससे जटिल कार्यों को आसानी से प्रबंधित और ट्रैक किया जा सके।

---

## LangGraph की विशेषताएँ (Features):

### 1. ग्राफ-आधारित आर्किटेक्चर:

- LangGraph वर्कफ्लो को **नोड्स** और **एजेस** के रूप में प्रस्तुत करता है।
- **नोड्स:** विभिन्न ऑपरेशन या कार्यों को प्रदर्शित करते हैं, जैसे समरीकरण, अनुवाद, या एपीआई कॉल।
- **एजेस:** कार्यों के बीच कनेक्शन को दिखाते हैं, यानी किस कार्य के बाद कौन सा कार्य निष्पादित होगा।

### 2. LangChain के साथ एकीकरण:

- LangGraph पूरी तरह से **LangChain** के साथ संगत है।
- LangChain के चेन, एजेंट्स, और मेमोरी को LangGraph में नोड्स के रूप में उपयोग किया जा सकता है।

### 3. विज़ुअलाइज़ेशन:

- यह वर्कफ्लो को **विज़ुअलाइज़** करने की सुविधा देता है, जिससे आप जटिल पाइपलाइनों को आसानी से समझ और डिबग कर सकते हैं।
- **उदाहरण:** एक पाइपलाइन जो समरीकरण के बाद अनुवाद और फिर डेटाबेस स्टोरेज करती है, उसे ग्राफ के रूप में देखा जा सकता है।

```python
from langgraph import GraphWorkflow

# नया वर्कफ्लो बनाएं
workflow = GraphWorkflow()

# नोड्स जोड़ें
workflow.add_node("Summarize", function=summarize_text)
workflow.add_node("Translate", function=translate_text)
workflow.add_node("Store", function=store_to_database)

# एजेस जोड़ें
workflow.add_edge("Summarize", "Translate")
workflow.add_edge("Translate", "Store")

# वर्कफ्लो चलाएं
workflow.run(input_data)
```

### 4. डायनामिक वर्कफ्लो:

- कार्यों को डायनामिक तरीके से व्यवस्थित किया जा सकता है, जिससे एआई सिस्टम अधिक फ्लेक्सिबल बनते हैं।
- **उदाहरण:** एक नोड इनपुट के आधार पर निर्णय ले सकता है कि अगले स्टेप में क्या करना है।

```python
def decision_node(input_data):
    if "translate" in input_data:
        return "Translate"
    return "Summarize"

workflow.add_node("Decision", function=decision_node)
workflow.add_edge("Decision", "Translate")
workflow.add_edge("Decision", "Summarize")
```

### 5. स्केलेबिलिटी (विस्तार क्षमता):

- बड़े और जटिल एआई वर्कफ्लो को प्रबंधित करने के लिए बनाया गया है।
- जैसे मल्टी-स्टेप ऑपरेशन्स, बाहरी एपीआई कॉल्स, और डेटाबेस इंटीग्रेशन।

### 6. डिबगिंग में सरलता:

- ग्राफ-आधारित प्रतिनिधित्व से त्रुटियों को ट्रेस करना और हल करना आसान हो जाता है।

---

## LangGraph का उपयोग:

### 1. जटिल पाइपलाइन्स:

- मल्टीमॉडल एआई टास्क्स को एक ही वर्कफ्लो में जोड़ना।
- **उदाहरण:** टेक्स्ट-टू-इमेज + इमेज एनालिसिस + रिपोर्ट जनरेशन।

```python
workflow.add_node("Generate Image", function=generate_image)
workflow.add_node("Analyze Image", function=analyze_image)
workflow.add_node("Generate Report", function=generate_report)
workflow.add_edge("Generate Image", "Analyze Image")
workflow.add_edge("Analyze Image", "Generate Report")
```

### 2. एआई सिस्टम डिबगिंग:

- वर्कफ्लो में हर नोड के आउटपुट को ट्रैक करना और त्रुटियों को समझना।

```python
workflow.enable_debug()
workflow.run(input_data)
```

### 3. डायनामिक एआई एजेंट्स:

- ऐसे एजेंट्स बनाना जो इनपुट के आधार पर अपनी रणनीतियाँ बदल सकें।

```python
from langchain.agents import initialize_agent, Tool
from langchain.llms import OpenAI

llm = OpenAI(temperature=0.7)

# एक टूल बनाएं
fetch_info_tool = Tool(
    name="FetchInfo",
    func=lambda x: f"यह {x} के बारे में जानकारी है।",
    description="यह उपयोगकर्ता द्वारा मांगी गई जानकारी देता है।"
)

# एजेंट बनाएं
agent = initialize_agent(
    tools=[fetch_info_tool],
    llm=llm,
    agent="zero-shot-react-description"
)

# एजेंट को रन करें
response = agent.run("मुझे क्लाइमेट चेंज के बारे में बताएं।")
print(response)
```

### 4. वर्कफ्लो प्रबंधन:

- मॉड्यूलर और रीयूज़ेबल एआई कंपोनेंट्स बनाना, जिन्हें अलग-अलग प्रोजेक्ट्स में उपयोग किया जा सके।

---

## LangChain और LangGraph में अंतर (Difference):

| **विशेषता**          | **LangChain**                   | **LangGraph**                                 |
| -------------------- | ------------------------------- | --------------------------------------------- |
| **आर्किटेक्चर**      | लिनियर और मॉड्यूलर वर्कफ्लो।    | ग्राफ-आधारित वर्कफ्लो।                        |
| **विज़ुअलाइज़ेशन**   | कोड और फंक्शन्स के रूप में।     | ग्राफ प्रतिनिधित्व (नोड्स और एजेस)।           |
| **जटिलता प्रबंधन**   | साधारण से जटिल वर्कफ्लो।        | उच्च जटिलता वाले मल्टी-स्टेप टास्क।           |
| **डिबगिंग**          | कोड आधारित डिबगिंग।             | ग्राफ-आधारित डिबगिंग, जहाँ हर स्टेप ट्रैक हो। |
| **उपयोग के क्षेत्र** | सीक्वेंशियल टास्क्स और एजेंट्स। | डायनामिक और जटिल एआई पाइपलाइन्स।              |

---

## LangGraph को कैसे इंस्टॉल करें?

Python में LangGraph का उपयोग करने के लिए:

```bash
pip install langgraph
```

---

## LangChain + LangGraph का उपयोग एक साथ:

LangGraph को LangChain के साथ इंटीग्रेट किया जा सकता है ताकि जटिल वर्कफ्लो को मॉड्यूलर और विज़ुअलाइज़ेबल बनाया जा सके।

**उदाहरण:**

- LangChain का इस्तेमाल प्रॉम्प्ट्स और मेमोरी के लिए करें।
- LangGraph का इस्तेमाल वर्कफ्लो को ग्राफ में प्रस्तुत करने के लिए करें।

```python
from langchain import PromptTemplate
from langgraph import GraphWorkflow

prompt = PromptTemplate("Translate the following: {text}")
workflow = GraphWorkflow()

workflow.add_node("Prompt", function=prompt.generate)
workflow.add_node("Translate", function=translate_text)
workflow.add_edge("Prompt", "Translate")
workflow.run({"text": "Hello, World!"})
```

---

अगर आप LangGraph का उपयोग किसी विशेष प्रोजेक्ट में करना चाहते हैं, तो मुझे बताइए, मैं पूरी मदद करूंगा! 😊
