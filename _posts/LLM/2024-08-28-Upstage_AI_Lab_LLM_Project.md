---
layout: single
title:  "Project_No.1_LLM"
categories: Project
tag: [Project]
toc: true
author_profile: false
---

<head>
  <style>
    table.dataframe {
      white-space: normal;
      width: 100%;
      height: 240px;
      display: block;
      overflow: auto;
      font-family: Arial, sans-serif;
      font-size: 0.9rem;
      line-height: 20px;
      text-align: center;
      border: 0px !important;
    }

    table.dataframe th {
      text-align: center;
      font-weight: bold;
      padding: 8px;
    }

    table.dataframe td {
      text-align: center;
      padding: 8px;
    }

    table.dataframe tr:hover {
      background: #b8d1f3; 
    }

    .output_prompt {
      overflow: auto;
      font-size: 0.9rem;
      line-height: 1.45;
      border-radius: 0.3rem;
      -webkit-overflow-scrolling: touch;
      padding: 0.8rem;
      margin-top: 0;
      margin-bottom: 15px;
      font: 1rem Consolas, "Liberation Mono", Menlo, Courier, monospace;
      color: $code-text-color;
      border: solid 1px $border-color;
      border-radius: 0.3rem;
      word-break: normal;
      white-space: pre;
    }

  .dataframe tbody tr th:only-of-type {
      vertical-align: middle;
  }

  .dataframe tbody tr th {
      vertical-align: top;
  }

  .dataframe thead th {
      text-align: center !important;
      padding: 8px;
  }

  .page__content p {
      margin: 0 0 0px !important;
  }

  .page__content p > strong {
    font-size: 0.8rem !important;
  }

  </style>
</head>

## 1. .env 환경 설정
처음으로 .env 폴더에서 제공받은 Upstage_API_Key를 입력하고 저장해준다



```python
import os
from dotenv import load_dotenv

load_dotenv()

MAX_MESSAGES_BEFORE_DELETION = 5
UPSTAGE_API_KEY = os.getenv("UPSTAGE_API_KEY")
```
## 2. chat_model.py
다음으로 chat_model.py를 만들어 입력과 출력을 가능하게 만들어주었다.



```python
# chat_model.py

from langchain_upstage import ChatUpstage
from langchain.chains.history_aware_retriever import create_history_aware_retriever
from langchain.chains.retrieval import create_retrieval_chain
from langchain.chains.combine_documents import create_stuff_documents_chain
from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from config import UPSTAGE_API_KEY

def setup_chat_model():
    chat = ChatUpstage(upstage_api_key=UPSTAGE_API_KEY)
    
    contextualize_q_system_prompt = """이전 대화 내용과 최신 사용자 질문이 있을 때, 이 질문이 이전 대화 내용과 관련이 있을 수 있습니다. 
    이런 경우, 대화 내용을 알 필요 없이 독립적으로 이해할 수 있는 질문으로 바꾸세요. 
    질문에 답할 필요는 없고, 필요하다면 그저 다시 구성하거나 그대로 두세요."""

    contextualize_q_prompt = ChatPromptTemplate.from_messages([
        ("system", contextualize_q_system_prompt),
        MessagesPlaceholder("chat_history"),
        ("human", "{input}"),
    ])

    qa_system_prompt = """질문-답변 업무를 돕는 보조원입니다. 
    질문에 답하기 위해 검색된 내용을 사용하세요. 
    답을 모르면 모른다고 말하세요. 
    친절한 말투를 사용해서 말하세오.
    10줄 정도로 정리해서 말하세요.

    ## 답변 예시
    📍답변 내용: 
    📍증거(참고 부분): 

    {context}"""

    qa_prompt = ChatPromptTemplate.from_messages([
        ("system", qa_system_prompt),
        MessagesPlaceholder("chat_history"),
        ("human", "{input}"),
    ])

    return chat, contextualize_q_prompt, qa_prompt

def create_rag_chain(chat, retriever, contextualize_q_prompt, qa_prompt):
    history_aware_retriever = create_history_aware_retriever(chat, retriever, contextualize_q_prompt)
    question_answer_chain = create_stuff_documents_chain(chat, qa_prompt)
    return create_retrieval_chain(history_aware_retriever, question_answer_chain)
```

## 3. doucument_loader.py
doucument_loader.py를 만들어 PDF 파일 업로드가 가능하도록 만들었다.



```python
from langchain_community.document_loaders import PyPDFLoader

def load_pdf(file_path):
    loader = PyPDFLoader(file_path)
    return loader.load_and_split()
```

Vector DB로 저장하기 위해 Embedding을 진행하였다.



```python
from langchain_chroma import Chroma
from langchain_upstage import UpstageEmbeddings

def create_vectorstore(pages):
    vectorstore = Chroma.from_documents(pages, UpstageEmbeddings(model="solar-embedding-1-large"))
    return vectorstore.as_retriever(k=2)
```

## 4. 시각화를 위한 streamlit
LLM 모델의 입력과 출력 그리고 PDF파일 업로드를 위한 시각화를 위해 streamlit을 이용하여 진행하였다.



```python
import streamlit as st
import base64

def reset_chat():
    st.session_state.messages = []
    st.session_state.context = None

def display_pdf(file):
    st.markdown("### PDF Preview")
    base64_pdf = base64.b64encode(file.read()).decode("utf-8")
    pdf_display = f"""<iframe src="data:application/pdf;base64,{base64_pdf}" width="400" height="100%" type="application/pdf"
                        style="height:100vh; width:100%"
                    >
                    </iframe>"""
    st.markdown(pdf_display, unsafe_allow_html=True)

def setup_sidebar():
    with st.sidebar:
        st.header("서류를 업로드 하세요.")
        uploaded_file = st.file_uploader("`.pdf` 파일을 고르세요.", type="pdf")
    return uploaded_file

def display_chat_messages():
    for message in st.session_state.messages:
        with st.chat_message(message["role"]):
            st.markdown(message["content"])

def get_user_input():
    return st.chat_input("Ask a question!")
```

## 5. main.py
마지막으로 전체 모듈을 구동하기 위해 main.py를 만들었다.



```python
import streamlit as st
import uuid
import tempfile
import os
import time
from config import MAX_MESSAGES_BEFORE_DELETION
from document_loader import load_pdf
from embedding import create_vectorstore
from chat_model import setup_chat_model, create_rag_chain
from ui_components import reset_chat, setup_sidebar, display_chat_messages, get_user_input
def main():
    st.title("QA Engine 1팀 챗봇")
    if "id" not in st.session_state:
        st.session_state.id = uuid.uuid4()
        st.session_state.file_cache = {}
    if "messages" not in st.session_state:
        st.session_state.messages = []
    uploaded_file = setup_sidebar()
    if uploaded_file:
        try:
            file_key = f"{st.session_state.id}-{uploaded_file.name}"
            with tempfile.TemporaryDirectory() as temp_dir:
                file_path = os.path.join(temp_dir, uploaded_file.name)
                with open(file_path, "wb") as f:
                    f.write(uploaded_file.getvalue())
                st.sidebar.write("Indexing your document...")
                if file_key not in st.session_state.get('file_cache', {}):
                    pages = load_pdf(file_path)
                    retriever = create_vectorstore(pages)
                    chat, contextualize_q_prompt, qa_prompt = setup_chat_model()
                    rag_chain = create_rag_chain(chat, retriever, contextualize_q_prompt, qa_prompt)
                    st.session_state.file_cache[file_key] = rag_chain
                st.sidebar.success("Ready to Chat!")
        except Exception as e:
            st.sidebar.error(f"An error occurred: {e}")
            st.stop()
    display_chat_messages()
    if prompt := get_user_input():
        if len(st.session_state.messages) >= MAX_MESSAGES_BEFORE_DELETION:
            del st.session_state.messages[:2]
        st.session_state.messages.append({"role": "user", "content": prompt})
        with st.chat_message("user"):
            st.markdown(prompt)
        with st.chat_message("assistant"):
            message_placeholder = st.empty()
            full_response = ""
            if uploaded_file:
                rag_chain = st.session_state.file_cache[file_key]
                result = rag_chain.invoke({"input": prompt, "chat_history": st.session_state.messages})
                with st.expander("Evidence context"):
                    st.write(result["context"])
                for chunk in result["answer"].split(" "):
                    full_response += chunk + " "
                    time.sleep(0.2)
                    message_placeholder.markdown(full_response + "▌")
                message_placeholder.markdown(full_response)
            else:
                full_response = "Please upload a PDF file first."
                message_placeholder.markdown(full_response)
        st.session_state.messages.append({"role": "assistant", "content": full_response})
if __name__ == "__main__":
    main()
```
