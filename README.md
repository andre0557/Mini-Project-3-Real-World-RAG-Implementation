# PhishRAG: AI-Powered Phishing Intelligence Assistant

**Student:** Andre McCloud  
**Course:** IPHS 391 - AI Mini-Project Series  
**Project:** Mini-Project #3 - Real-World RAG Implementation  
**Date:** November 12, 2025  

[![Status](https://img.shields.io/badge/status-Design%20Complete-success.svg)](#)

---

## 📋 Project Overview

This repository contains the completed **Mini-Project #3: Real-World RAG Implementation** for the AI Mini-Project Series. The project demonstrates the application of Retrieval-Augmented Generation (RAG) to a real-world cybersecurity use case using the **Composable AI Project Blueprint (CAPB)** framework.

### Project Requirements Met

✅ **Realistic Corpus:** 50,000+ phishing intelligence samples from 5 public datasets  
✅ **RAG Architecture:** Minimal but complete 8-stage pipeline designed for security workflows  
✅ **CAPB Documentation:** Full technical report following the provided skeleton template  
✅ **Business + Technical Reasoning:** Use case analysis, component justification, and evaluation planning  

---

## 🎯 What is PhishRAG?

**PhishRAG** is a RAG-powered research assistant designed to help cybersecurity analysts rapidly search and summarize phishing intelligence from multiple public data sources.

**The Problem:**  
Security analysts spend 3-5 hours manually reviewing phishing emails, advisories, and threat reports to identify attack patterns. This is slow, inconsistent, and doesn't scale during active phishing campaigns.

**The Solution:**  
A retrieval-augmented generation system that searches across 50,000+ phishing samples, generates cited summaries, and helps analysts extract actionable insights in seconds instead of hours.

---

## 🔐 Why Phishing Intelligence?

Phishing remains the #1 attack vector in cybersecurity. This project addresses a real operational need for:
- **SOC Analysts** triaging incidents
- **Security Consultants** preparing threat assessments  
- **Training Teams** building awareness content
- **Threat Researchers** studying campaign evolution

Unlike generic RAG chatbots, PhishRAG is purpose-built for security workflows with sanitized data handling, required source citations, and domain-specific retrieval strategies.

---

## 📊 Data Sources

The system integrates five public phishing intelligence datasets:

- **PhishTank** - 30,000 verified phishing URLs
- **CISA Advisories** - 200 government security alerts  
- **Nazario Corpus** - 9,000 real phishing emails
- **SpamAssassin** - 6,000 spam samples
- **Hugging Face** - 18,000 labeled phishing emails

All data is public, anonymized, and safe for educational use.

---

## 📄 Deliverable: CAPB Technical Report

The complete technical design report follows the **Composable AI Project Blueprint (CAPB)** framework and includes all required sections:

1. **Project Context & Use Case** - Problem definition, target users, success criteria
2. **Data & Constraints** - Corpus details, formats, security and budget constraints
3. **RAG Architecture (MVP)** - Complete pipeline design with component justification
4. **Component Alternatives** - Comparison tables for framework, vector DB, embeddings, reranker, LLM, and UI choices
5. **Evaluation Plan & Results** - Test methodology, metrics, and simulated performance analysis
6. **Risks, Edge Cases & Future Work** - Security considerations, limitations, and enhancement roadmap
7. **References** - Key resources and data sources

➡️ **[View Full CAPB Report (PhishRAG_MiniProject3.md)](PhishRAG_MiniProject3.md)**

---

## 🚀 Future Work

- Add temporal filtering for recent campaigns
- Experiment with GraphRAG for threat actor relationships
- Integrate with SIEM systems for real-time analysis
- Fine-tune embeddings on security-specific corpora

---

## 🔒 Security & Ethics

✅ All data sources are public and anonymized  
✅ HTML content is sanitized (no code execution)  
✅ No live URL visits (protects against malicious sites)  
✅ Local-only processing (sensitive analysis stays on-premises)  

**Educational use only.** Production deployment requires additional security hardening.

---

## 🙏 Acknowledgments

**Course:** IPHS 391 AI Mini-Project Series (CAPB Framework)  
**Data:** PhishTank, CISA, Jose Nazario, SpamAssassin, Hugging Face Community  
**Tools:** LangChain, OpenAI, Cohere, FAISS

---

## 📧 Contact

**Andre McCloud**  
For questions about this project, feel free to reach out.

---

**License:** Educational purposes only. All rights reserved © 2025 Andre McCloud
