# Program1
import gensim.downloader as api

print("Loading pre-trained word vectors...")
wv = api.load("word2vec-google-news-300")

# Word analogy
def ex(a,b,c):
    try:
        r = wv[a]-wv[b]+wv[c]
        print(f"\nWord Relationship: {a} - {b} + {c}\nMost similar words:")
        for w,s in [(x,y) for x,y in wv.similar_by_vector(r,10) if x not in {a,b,c}][:5]:
            print(f"{w}: {s:.4f}")
    except: print("Error")

# Similarity
def sim(a,b):
    try: print(f"\nSimilarity between '{a}' and '{b}': {wv.similarity(a,b):.4f}")
    except: print("Error")

# Most similar
def ms(w):
    try:
        print(f"\nMost similar words to '{w}':")
        for x,s in wv.most_similar(w,5): print(f"{x}: {s:.4f}")
    except: print("Error")

# Calls
[ex(*x) for x in [("king","man","woman"),("paris","france","germany"),("apple","fruit","carrot")]]
[sim(*x) for x in [("cat","dog"),("computer","keyboard"),("music","art")]]
[ms(x) for x in ["happy","sad","technology"]]

# Program2
!pip install gensim scikit-learn matplotlib

import gensim.downloader as api, numpy as np, matplotlib.pyplot as plt
from sklearn.decomposition import PCA

print("Loading pre-trained Word2Vec model...")
wv = api.load("word2vec-google-news-300")
print("Model loaded successfully!\n")

# Similar words
for w in ["computer","learning"]:
    try:
        print(f"\nTop 5 semantically similar words to '{w}':")
        [print(f"{x} : {s:.4f}") for x,s in wv.most_similar(w,5)]
    except:
        print(f"Word '{w}' not found")

# PCA
words = ["computer","software","hardware","algorithm","data",
         "network","programming","machine","learning","artificial"]

r = PCA(2).fit_transform(np.array([wv[w] for w in words]))

plt.figure()
for i,w in enumerate(words):
    plt.scatter(*r[i]); plt.text(r[i,0]+0.02, r[i,1]+0.02, w)
plt.grid(); plt.show()


# Program3
!pip install gensim scikit-learn matplotlib

from gensim.models import Word2Vec
from sklearn.manifold import TSNE
import matplotlib.pyplot as plt
import numpy as np, re

# Data
corpus = ["The patient was diagnosed with diabetes and hypertension",
"MRI scans reveal abnormalities in brain tissue",
"Treatment involves antibiotics and regular monitoring",
"Symptoms include fever fatigue and muscle pain",
"Vaccine is effective against viral infections",
"Doctors recommend physical therapy for recovery",
"Clinical trial results were published in journal",
"Surgeon performed minimally invasive procedure",
"Prescription includes pain relievers and anti inflammatory drugs",
"Diagnosis confirmed rare genetic disorder"]

# Train
proc = [re.sub(r'[^a-z\s]', '', s.lower()).split() for s in corpus]
m = Word2Vec(proc, vector_size=100, window=3, min_count=1, epochs=100)

# Similar words
for w in ["treatment","vaccine"]:
    print(f"\n{w} →", [x for x,_ in m.wv.most_similar(w,5)])

# t-SNE
w = m.wv.index_to_key
v = np.array([m.wv[i] for i in w])
r = TSNE(n_components=2, perplexity=5).fit_transform(v)

plt.figure()
plt.scatter(r[:,0], r[:,1])
for i, x in enumerate(w): plt.text(r[i,0], r[i,1], x)
plt.show()

# Program4
import gensim.downloader as api
from transformers import pipeline

# Load embeddings + generator
print("Loading GloVe word embeddings...")
wv = api.load("glove-wiki-gigaword-100")

print("\nLoading distilgpt2 model (lighter version of GPT-2)...")
gen = pipeline("text-generation", model="distilgpt2")

# Enrich prompt
def enrich(p, k, n=3):
    try:
        sims = [w for w, _ in wv.most_similar(k, topn=n)]
        print(f"\nSimilar words for '{k}': {sims}")
        return p + " Include related concepts such as " + ", ".join(sims) + "."
    except:
        print("Keyword not found in vocabulary.")
        return p

# Generate text
def generate(p):
    return gen(p, max_new_tokens=100, do_sample=True, temperature=0.8,
               top_k=50, top_p=0.9, repetition_penalty=1.2,
               no_repeat_ngram_size=3, pad_token_id=50256)[0]['generated_text']

# Prompts
orig = "Explain the responsibilities of a king in a kingdom."
print("\nOriginal Prompt:\n", orig)

enriched = enrich(orig, "king")
print("\nEnriched Prompt:\n", enriched)

# Generate
print("\nGenerating response for original prompt...")
res1 = generate(orig)

print("\nGenerating response for enriched prompt...")
res2 = generate(enriched)

# Output
print("\n--- Original Response ---\n", res1)
print("\n--- Enriched Response ---\n", res2)

print("\nComparison:")
print("Original Response Length:", len(res1))
print("Enriched Response Length:", len(res2))


# Program5
import gensim.downloader as api, random

print("Loading word vectors...")
wv = api.load("glove-wiki-gigaword-100")
print("Model loaded successfully!\n")

w = input("Enter a seed word: ")

try:
    words = [x for x, _ in wv.most_similar(w, 5)]
    print("\nSimilar words:", words)

    random.shuffle(words)

    para = (
        f"The word '{w}' often reminds people of {words[0]} and {words[1]}. "
        f"In many discussions, {words[2]} is closely connected with {w}. "
        f"People may also think about {words[3]} when talking about {w}. "
        f"Together, ideas like {words[4]} help explain the broader meaning of '{w}'."
    )

    print("\nGenerated Paragraph:\n")
    print(para)

except:
    print("Word not found in vocabulary. Please try another word.")

# Program6
from transformers import pipeline

print("\nLoading Sentiment analysis model")

sa=pipeline("sentiment-analysis")

reviews=[
    "Product was good",
    "Worst Product ever"
]

print("Customer feedback: \n")

for r in reviews:
    res=sa(r)[0]
    print("Result : ",res)
    print("\nLabel: ",res['label'])
    print("\nScore: ",round(res['score'],4))

# Program7
!pip uninstall -y transformers
!pip install transformers==4.41.2 torch sentencepiece

// restart session


from transformers import pipeline

print("Loading Summarization Model/BART: ")

summ=pipeline("summarization",model="facebook/bart-large-cnn")

def summarize(t):
    t=" ".join(t.split())
    mx=min(len(t)//3,150)
    mn=max(30,mx//3)
    return summ(t,max_length=mx,min_length=mn,do_sample=False)[0]['summary_text']

text="""Artificial Intelligence (AI) is a rapidly evolving field of computer science..."""

res=summarize(text)

print("\n Original Text: ",text)
print("\n Summarize Text: ",res)

# Program8
# Install once
!pip install langchain-cohere cohere

from langchain_cohere import ChatCohere
import getpass

# Input text (instead of file for simplicity)
text = "Teaching is a process of sharing knowledge and helping others learn."

# Model
llm = ChatCohere(cohere_api_key=getpass.getpass("Enter API key: "))

# Prompt + Output
res = llm.invoke(f"Summarize this text and give 3 key points + sentiment:\n{text}").content

print(res)

# Progrma9
//Install This library
!pip install wikipedia-api pydantic


// code
from pydantic import BaseModel
from typing import Optional, List
import wikipediaapi
from IPython.display import display
import ipywidgets as widgets

# Schema
class Inst(BaseModel):
    founder: Optional[str]
    founded: Optional[str]
    summary: Optional[str]

# Fetch + Parse
def get_data(name):
    page = wikipediaapi.Wikipedia(user_agent="GenAI-Lab/1.0 (vinay@student.com)", language="en").page(name)
    if not page.exists(): raise ValueError("Page not found!")

    txt = page.text.lower().split('\n')

    fndr = next((l for l in txt if "founder" in l), None)
    fndd = next((l for l in txt if "founded" in l), None)
    summ = '. '.join(page.summary.split('. ')[:4])

    return Inst(founder=fndr, founded=fndd, summary=summ)

# UI
box = widgets.Text(description="Institution:")
btn = widgets.Button(description="Fetch")

def click(_):
    try:
        d = get_data(box.value)
        print(f"Founder: {d.founder or 'N/A'}")
        print(f"Founded: {d.founded or 'N/A'}")
        print(f"Summary: {d.summary}")
    except Exception as e:
        print("Error:", e)

btn.on_click(click)
display(box, btn)

prohgram 10

!pip install langchain cohere langchain-cohere wikipedia-api pydantic

import getpass, wikipediaapi
from langchain_core.prompts import PromptTemplate
from langchain_cohere import ChatCohere
from pydantic import BaseModel
from typing import Optional
from IPython.display import display
import ipywidgets as widgets

# API + Model
llm = ChatCohere(cohere_api_key=getpass.getpass("API Key: "), model="command")

# Fetch IPC
ipc = wikipediaapi.Wikipedia(
    user_agent="IPCChatbot/1.0 (student@lab.com)", language='en'
).page("Indian Penal Code").summary

# Schema
class R(BaseModel):
    section: Optional[str]
    explanation: str

# Prompt
p = PromptTemplate(
    input_variables=["c","q"],
    template="Use:\n{c}\nQ:{q}\nFormat:\nSection:\nExplanation:"
)

# Chat
def ask(q):
    res = llm.invoke(p.format(c=ipc,q=q))
    sec = None
    exp = res
    if "Section:" in res:
        x = res.split("Explanation:")
        sec = x[0].replace("Section:","").strip()
        exp = x[1].strip() if len(x)>1 else ""
    return R(section=sec, explanation=exp)

# UI
t = widgets.Text(description="You:")
b = widgets.Button(description="Ask")

def click(_):
    try:
        r = ask(t.value)
        print(f"\nSection: {r.section or 'N/A'}\nExplanation: {r.explanation}")
    except Exception as e:
        print("Error:", e)

b.on_click(click)
display(t,b)
