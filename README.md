```markdown
# UPSC Essay Evaluator using LangGraph

A Python-based evaluator for UPSC essays, built using LangGraph and Gemini AI. This project demonstrates how to structure an essay evaluation workflow as a stateful graph, assessing language quality, depth of analysis, clarity of thought, and providing summarized feedback with an average score.

## Project Overview

This project utilizes langgraph to create a directed graph that:

1. Initializes State: Accepts the essay text.

2. Evaluates Language: Executes a node to assess grammar, vocabulary, structure, coherence, and provides feedback with a score out of 10.

3. Evaluates Analysis: Executes a node to assess the depth of analysis and provides feedback with a score out of 10.

4. Evaluates Clarity of Thought: Executes a node to assess the clarity of thought and provides feedback with a score out of 10.

5. Final Evaluation: Summarizes all feedbacks and calculates the average score.

## Features

* State Management: Uses TypedDict to maintain a consistent state across nodes, including feedbacks and scores.

* Modular Logic: Separate nodes for each evaluation criterion and final summarization.

* AI Integration: Leverages Gemini AI model for structured evaluations.

* Graph Visualization: Includes code to render the graph structure using Mermaid.

## Technical Stack

* Language: Python

* Orchestration: LangGraph

* AI Model: LangChain Google GenAI (Gemini)

* Data Handling: typing.TypedDict, Pydantic

* Environment: python-dotenv

## Installation

1. Clone the repository:

   ```
   git clone https://github.com/your-username/upsc-essay-langgraph.git
   cd upsc-essay-langgraph
   ```

2. Install dependencies:

   ```
   pip install langgraph langchain-google-genai pydantic python-dotenv
   ```

3. Set up your Google API key in a `.env` file:

   ```
   GOOGLE_API_KEY=your_api_key_here
   ```

## How It Works

### The State

The UPSCState keeps track of the essay, individual feedbacks, scores, overall feedback, and average score:

```python
class UPSCState(TypedDict):
    essay: str
    language_feedback: str
    analysis_feedback: str
    clarity_feedback: str
    overall_feedback: str
    individual_scores: Annotated[list[int], operator.add]
    avg_score: float
```

### The Graph Logic

The workflow follows parallel paths from START to evaluations, then converges to final evaluation:

* START → evaluate_language → final_evaluation → END
* START → evaluate_analysis → final_evaluation → END
* START → evaluate_thought → final_evaluation → END

## Usage

Run the script to see the output for a sample input:

```python
initial_state = {
    'essay': '''Artificial intelligence (AI) has evolved rapidly over the past few decades, transforming from simple rule-based programs into advanced systems capable of learning, reasoning, and making decisions. In the future, the evolution of AI is expected to bring even more profound changes to society, reshaping the way humans live, work, and interact with technology. As computing power increases and data becomes more abundant, AI systems will grow smarter, faster, and more efficient, enabling solutions to complex global challenges.

One major direction in the future evolution of AI is greater adaptability and personalization. AI systems will be able to understand individual preferences, emotions, and contexts more accurately. In healthcare, AI may predict diseases before symptoms appear, assist doctors in diagnosis, and design personalized treatment plans. In education, intelligent tutors could adapt lessons to each student’s learning style and pace, making education more inclusive and effective. Such personalized AI systems have the potential to significantly improve human well-being.

Another important aspect of AI’s future is its integration with physical systems through robotics and the Internet of Things. Smart machines and robots will be capable of performing tasks that are dangerous, repetitive, or physically demanding, such as disaster rescue, deep-sea exploration, and precision manufacturing. This will increase productivity and safety across industries. At the same time, AI-powered smart cities may optimize traffic flow, energy consumption, and public services, leading to more sustainable urban living.

Despite these benefits, the future evolution of AI also raises important ethical and social concerns. Issues such as job displacement, data privacy, bias, and accountability must be addressed carefully. While AI may automate certain jobs, it will also create new roles that require human creativity, critical thinking, and emotional intelligence. Therefore, continuous skill development and reskilling will be essential for the workforce. Governments, organizations, and educators must work together to ensure a smooth transition.

Ethical AI development will play a crucial role in shaping the future. Transparency, fairness, and explainability should be built into AI systems so that users can trust and understand their decisions. Clear regulations and global cooperation will be needed to prevent misuse of AI technologies and to ensure they are aligned with human values. Responsible governance can help balance innovation with safety.

In conclusion, the evolution of AI in the future holds immense promise as well as significant responsibility. If developed and used wisely, AI can enhance human capabilities, solve complex problems, and contribute to sustainable growth. By focusing on ethical principles, inclusive policies, and human-centered design, society can ensure that AI becomes a powerful tool for improving the quality of life for people around the world.

'''
}
final_state = workflow.invoke(initial_state)
print(final_state)
```

## Visualization

The project includes a utility to display the graph structure:

```python
from IPython.display import Image
Image(workflow.get_graph().draw_mermaid_png())
```
```
