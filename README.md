# Course Recommendation​ System
**Authors:** Jiaxin Su
## Introduction
Besides being a research institution, UCSB also offers its students a liberal arts education through a wide variety to interesting courses in different subject areas. Especially with the holistic admission process, bringing in well-rounded individuals with many different interests. However, once they’ve been admitted, students are then left to rely on academic advising, hear the words of mouth from their peers, Reddit, or randomly scouring through GOLD minutes before their course registration pass time to find interesting courses outside of their major. In other words, there isn’t a way for students to get tailored list of courses that match their interests at UCSB. For this project, my goal is to streamline the course search process by developing a course recommendation system that suggests UCSB courses to students based on their interests and provides clear, well-supported reasons for each recommendation.


## Overview
I’ll be working with two separate data sets. The first data set contains information about all courses offered at UCSB and the second data set will be about students’ and their extracurricular activities. For UCSB courses information, we plan on using an API. Once I’ve gathered this data, I’ll store it in a csv file and use it build the recommendation system. As for the students’ data set, I’ll use data from “Students Extracurricular Info”, obtained via Kaggle, as the real UCSB student course interest and course history are hard to get.


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


### How to Run

