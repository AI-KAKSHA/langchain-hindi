
# LangChain Framework

LangChain एक फ्रेमवर्क है जो **Large Language Models (LLMs)** के साथ काम करने के लिए डेवेलपर्स को सक्षम बनाता है। इसका उद्देश्य एआई आधारित वर्कफ्लो को ज्यादा प्रभावी और मॉड्यूलर बनाना है। LangChain को **पाइथन** और **जावास्क्रिप्ट** में उपयोग किया जा सकता है।

---

## LangChain की विशेषताएँ (Features):

### 1. **प्रॉम्प्ट इंजीनियरिंग (Prompt Engineering):**
- प्रॉम्प्ट्स को मैनेज और डिजाइन करने के लिए टूल्स।
- सटीक और उपयोगी आउटपुट के लिए कस्टम प्रॉम्प्ट तैयार करें।

```python
from langchain.prompts import PromptTemplate

prompt = PromptTemplate(
    input_variables=["name"],
    template="नमस्ते {name}, क्या मैं आपकी मदद कर सकता हूँ?"
)
output = prompt.format(name="राज")
print(output)
```

### 2. **चेन (Chains):**
- मल्टी-स्टेप वर्कफ्लो तैयार करने के लिए।

```python
from langchain.chains import LLMChain
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate

llm = OpenAI(temperature=0.7)
prompt = PromptTemplate(
    input_variables=["topic"],
    template="मुझे {topic} के बारे में जानकारी चाहिए।"
)
chain = LLMChain(llm=llm, prompt=prompt)
response = chain.run("आर्टिफिशियल इंटेलिजेंस")
print(response)
```

### 3. **एजेंट्स (Agents):**
- डायनामिक वर्कफ्लो तैयार करने के लिए।
- एजेंट्स अपने आउटपुट के आधार पर निर्णय लेते हैं।

```python
from langchain.agents import initialize_agent, Tool
from langchain.llms import OpenAI

llm = OpenAI(temperature=0.7)
tool = Tool(
    name="FetchInfo",
    func=lambda x: f"यह {x} के बारे में जानकारी है।",
    description="यह उपयोगकर्ता द्वारा मांगी गई जानकारी देता है।"
)
agent = initialize_agent(tools=[tool], llm=llm, agent="zero-shot-react-description")
response = agent.run("मुझे क्लाइमेट चेंज के बारे में बताएं।")
print(response)
```

### 4. **मेमोरी (Memory):**
- पिछली बातचीत को याद रखने के लिए।

```python
from langchain.memory import ConversationBufferMemory

memory = ConversationBufferMemory()
memory.save_context({"input": "आपका नाम क्या है?"}, {"output": "मेरा नाम LangChain है।"})
conversation = memory.load_memory_variables({})
print(conversation)
```

---

## LangChain के टूल्स और लाइब्रेरी (Tools and Libraries):

1. **Prompts:**
   - कस्टम प्रॉम्प्ट्स को डिजाइन और मैनेज करने के लिए।
   - Dynamic inputs और reusable templates।

2. **Chains:**
   - मल्टी-स्टेप वर्कफ्लो को सरल बनाने के लिए।
   - Sequential और Parallel चेन को मैनेज करने की क्षमता।

3. **Agents:**
   - Decision-making workflows को simplify करने के लिए।
   - Tools और APIs के साथ dynamic interaction।

4. **Memory:**
   - एआई सिस्टम्स को पिछली बातचीत याद रखने की क्षमता देता है।

5. **Tool Integration:**
   - External APIs, databases और file systems को integrate करने के लिए।

---

## LangChain की Integration क्षमता (Integration Capacity):

LangChain कई बाहरी टूल्स और प्लेटफ़ॉर्म्स के साथ integration कर सकता है:

### 1. **API Integration:**
   - किसी भी RESTful API को integrate कर सकते हैं।
   - Example: OpenWeather, Google Maps API।

```python
from langchain.tools import Tool

def fetch_weather(location):
    # उदाहरण के लिए, API कॉल से डेटा प्राप्त करें।
    return f"{location} का मौसम अच्छा है।"

weather_tool = Tool(
    name="WeatherTool",
    func=fetch_weather,
    description="मौसम की जानकारी प्राप्त करें।"
)
```

### 2. **Database Integration:**
   - SQL और NoSQL डेटाबेस के साथ जुड़ने की क्षमता।
   - Example: MySQL, MongoDB।

```python
from langchain.tools import Tool
import sqlite3

def fetch_from_db(query):
    connection = sqlite3.connect("example.db")
    cursor = connection.cursor()
    cursor.execute(query)
    return cursor.fetchall()

sql_tool = Tool(
    name="DatabaseQueryTool",
    func=fetch_from_db,
    description="SQL क्वेरी चलाने के लिए।"
)
```

### 3. **Vector Databases:**
   - Pinecone, Chroma, और Weaviate जैसे vector databases के साथ integration।

### 4. **Pre-trained Models:**
   - OpenAI, Hugging Face, और अन्य मॉडल्स का उपयोग।

```python
from langchain.llms import HuggingFaceHub

llm = HuggingFaceHub(repo_id="gpt2")
response = llm("मुझे एक कहानी सुनाओ।")
print(response)
```

### 5. **File Systems:**
   - लोकल और क्लाउड फाइल सिस्टम्स से जुड़ने की क्षमता।
   - Example: AWS S3, Google Drive।

---

## LangChain का उपयोग:

### 1. **प्रश्न-उत्तर सिस्टम:**
- दस्तावेज़ों के आधार पर सवालों का जवाब देने के लिए।

### 2. **चैटबॉट:**
- उपयोगकर्ता के साथ संवाद करने वाले स्मार्ट चैटबॉट बनाने के लिए।

### 3. **डेटा प्रोसेसिंग:**
- डेटा को प्रॉसेस और एनालाइज करने के लिए मल्टी-स्टेप चेन तैयार करें।

### 4. **एआई एजेंट्स:**
- स्वायत्त निर्णय लेने वाले एआई एजेंट्स बनाने के लिए।

---

## LangChain को कैसे इंस्टॉल करें?

```bash
pip install langchain
pip install openai
```

---

अगर आपको LangChain के साथ किसी विशेष प्रोजेक्ट में मदद चाहिए, तो मुझे बताइए! 😊
