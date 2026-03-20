# Course Recommendation​ System
**Authors:** Jiaxin Su
## Introduction
Besides being a research institution, UCSB also offers its students a liberal arts education through a wide variety of interesting courses across many subject areas. Especially with its holistic admissions process, the university brings in well-rounded individuals with diverse academic and personal interests. However, once admitted, students are often left to rely on academic advising, word of mouth from peers, Reddit, or last-minute searches through GOLD before their registration pass time to find interesting courses outside of their major. In other words, there is not currently an easy way for students to receive a tailored list of UCSB courses that matches their interests. For this project, my goal is to streamline the course search process by developing a course recommendation system that suggests UCSB courses to students based on their interests and provides clear, well-supported reasons for each recommendation. To extend this system, I also incorporate a large language model (LLM) as a reranking and explanation layer: after retrieving the most relevant candidate courses, the LLM refines the final recommendations and generates more personalized, natural-language justifications for why each course may be a good fit.

## Overview
I’ll be working with two separate datasets. The first dataset contains information about UCSB courses, and the second contains student profile information, including their interests and extracurricular activities. For UCSB course information, I originally planned to use an API, but for this project I start with a cleaned UCSB course dataset stored as a CSV file and use it to build the recommendation system. As for the student dataset, I use Students Extracurricular Info from Kaggle, since real UCSB student course-interest and course-history data are difficult to access. To extend the recommender, I also incorporate LLM as a reranking and explanation layer: after the system retrieves the most relevant candidate courses, LLM helps refine the final recommendations and provides more personalized reasons for why each course may be a good fit.

## Dataset

### Source
(1) **student_data.csv**
The student dataset is from [Kaggle](https://www.kaggle.com/datasets/kamakshilahoti/student-extracurriculars-info). This dataset contains information about students, their academic interests, extracurricular activities, skills, location, year of study, major, GPA, languages spoken, club memberships, and research interests. 

Here's a description of each field in the dataset:
- StudentID: A unique identifier for each student.
- Name: The name of the student.
- AcademicInterest: The field of study or academic interest of the student.
- ExtracurricularActivities: Extracurricular activities the student is involved in.
- Skills: Skills possessed by the student.
- Location: The city or location where the student is currently based.
- YearOfStudy: The year of study for the student (e.g., Freshman, Junior, Senior, Graduate).
- Major: The major or field of study that the student is pursuing.
- GPA: The Grade Point Average of the student.
- Languages: Languages spoken or known by the student.
- ClubMemberships: Memberships in various clubs or organizations.
- ResearchInterests: Research interests or specialization of the student.

(2) **Fall2024_Courses.csv**
The course description data is acquired through [Academic Curriculum v3.0](https://developer.ucsb.edu/content/academic-curriculums#/Classes/Classes_GetClassesAsync). Only the course description and other course information in fall 2024 is loaded for simplicity of this project. 
The final course csv includes:
- quarter: the academic quarter in which the course was offered
- courseId: unique identifier for a course, like ANTH      3
- title: The full title of each course
- dept_code: The department where the course happen with its code.
- description: A brief summary of the course’s core content, typically written in a few sentences.
- subject_area: the subject area of the course.
- obj_level: Indicating if the course is graduate(G) or undergraduate level (U)
- college: the college under which the course is offered.
- units: The number of units assigned to the course
- instruction_type1: the primary instruction type of the course, such as seminar (SEM) or lecture (LEC).
- online_course: A binary variable indicating if the course is offered online.

**Dimensions:** The dataset includes 2,266 courses offered in Fall 2024 and 1,000 students.


## How to Run
### 1. Clone and install dependencies

```bash
git clone <your-repo-url>
cd <repo-name>
pip install -r requirements.txt
```
### 2. API Key
You need an OpenRouter API key (or OpenAI key) to run this project. Get the shared key, or create a free account at [openrouter.ai](https://openrouter.ai).
Create a `.env` file:
```
OPENROUTER_API_KEY=your_key_here
```
### 3. Run the notebook
```
final.ipynb
```
## Dependencies

- Python 3.11+
- OpenRouter or OpenAI API key
- Key libraries: `openai-agents`, `pydantic`, `rank-bm25`, `openai`, `pandas`, `python-dotenv`

See `requirements.txt` for the full list.

## Design Decisions & Trade-offs
**BM25 retrieval as the first-stage recommender:** I kept BM25 as the retrieval backbone because it is fast, interpretable, and already worked well to recommend top 5 courses. The tradeoff is that BM25 depends on lexical overlap, so it may miss semantically relevant matches phrased differently.

**Weighted scoring across student fields:** I assigned higher weights to academic interest, major, and research interests because these seemed most directly related to course relevance. This improves alignment with academic goals, but the weighting scheme is heuristic rather than learned from real enrollment data.

**Year-of-study boosting:** I boosted lower-division courses for freshmen/sophomores and mid-level courses for juniors/seniors to make recommendations more realistic. This adds useful structure, but it is a simple rule and does not account for exceptions such as advanced early students.

**Filtering out 99 and 199 courses:** I excluded 99 and 199 courses because they often represent independent study or research assistant placements that appeared too frequently across students. This improves recommendation quality for general course planning, though it may hide genuinely relevant research opportunities for some students.

**LLM reranking instead of full-catalog LLM recommendation:** I used the LLM only after BM25 retrieval, so it reranks a shortlist rather than searching the entire catalog. This keeps the system more efficient and grounded, but it means the final recommendations are limited by the quality of the BM25 candidate pool.

**OpenRouter/OpenAI-compatible chat model for explanations:** I integrated an LLM to refine the top candidate courses and generate personalized explanations. This makes the system more flexible and user-friendly.

## Example Output

**Selected student:** Student 960, a senior Biology major interested in Physics and Sustainable Agriculture.
**Pipeline:** The system first retrieves the top 5 courses with the highest BM25 scores, then sends them to the LLM to select and explain the top 3 recommendations.

| Rank | Course ID | Title | BM25 Score |
|------|-----------|-------|------------|
| 1 | ESM 224 | SSTNBL WTR RES MGMT | 26.5898 |
| 2 | EEMB 188RE | CONSERV RESTOR SEM | 24.7586 |
| 3 | ME 154 | STRUCTURES | 27.6125 |

**LLM explanation:**  
- **ESM 224** was recommended because it aligns with the student’s sustainability-related research interests.  
- **EEMB 188RE** fits the student’s biology background and conservation-related themes.  
- **ME 154** the introductory course in structural analysis and design can enhance the student's understanding of engineering principles, which is beneficial given their interest in physics and leadership skills in project design."

## Citations

- **OpenAI Agents SDK:** https://github.com/openai/openai-agents-python
- **Academic Curriculum v3.0 API:** https://developer.ucsb.edu/content/academic-curriculums#/Classes/Classes_GetClassesAsync
- **“Students Extracurricular Info” Data Set:** https://www.kaggle.com/datasets/kamakshilahoti/student-extracurriculars-info
