# Synthetic_Polling
Using GPT-3.5 and GPT-4 to simulate public opinion

# TL;DR: 
This project uses language model-based systems to predict the outcomes of polls as an alternative to traditional polling methods, which can be costly and often suffer from bias and demographic representation issues. The experiment simulates a poll by creating detailed personas representative of a target population (in this case, Montana residents) and using these personas as prompts to a large language model (LLM). These models are then asked a series of questions, and their responses are compared with the results of a real poll.

Issues arise with the existing models like GPT-3.5-turbo showing bias and always responding with certain answers regardless of the persona's characteristics. GPT-4 seems to perform better, offering more nuanced answers aligned with the demographic profiles, but comes with a significantly higher cost due to increased token cost.

Although the technology shows promise, extensive testing on model bias needs to be done to ensure valid and representative results. The estimated cost of a simulated poll using GPT-4, around $80, is significantly cheaper than traditional polls, which could cost between $20,000 and $40,000. Future work includes running a full simulation with GPT-4 and better structuring of prompts and personas for improved feedback.



# Simulated Polling Write Up
# Inspiration

- I feel one of the true powers of LLM systems is going to be the ability to predict the outcomes actions would have, and when AGI/ACE systems are built to scale, they will be able to understand what the consequences to a given action would be down to a granular level.
- An example of this would be supposed a law or infastructure change was going to be put in place, government officials would be thinking of the outcomes and effects it would have on a constituency
- They are only people so they can cannot think of everything, so a good way to get information is a poll
    - Issues with polling
        - Very expensive, $20-40 per person usually.  Statistics say 1000 people is good population size for 95% confidence, so $20,000-$40,000 per poll on every issue could get very expensive
        - Getting a proper stratification can be very hard as to get people to answer they need to answer the phone/want to be interviewed.  You may not be able to get people from every demographic.
        - People may not be educated on an issue and then just answer with what they feel they should.
        - People may not provide truthful answers to the poller
- Being able to simulate this would solve many of the issues
    - Will be much cheaper, can stratify however we see fit to best model a population, can provide contextual knowledge in a context window, and will always answer truthfully if prompted
- Issues:
    - How do we know if the answers provided are actually representative of a population?
        - Underlying bias in model?  Understanding emotions and how personal experience may shift someones beliefs to not fall directly into a party line.

# Proposed Solution

- Find a poll done on real people
- Create personas that are representative of the population of the poll
- Then using these personas as the system prompt to a LLM.  Then ask questions to each persona to get to see if the model can answer questions as that “person”.
- Compare the results of the simulated poll and real poll
- If the answers line up, then would show a possibility that this an avenue worth pursuing

# Goal:

- See the possibilities of simulated polling using generated personas to answer questions
- Similar to the Paper
    - ****Language Models Trained on Media Diets Can Predict Public Opinion****
    - https://arxiv.org/abs/2303.16779v1
    - This paper fine tuned a model by feeding it a “Media Diet”, where the media diet is news from a specific news source, say Fox News.  Then this model was asked questions and it answered them as a “Fox News Viewer”, and the results were compared to responses of human Fox News Viewers.  Results showed that this methodology worked very well for predicting what a viewer of a specific news source would respond
- Issues with this methodology
    - This methodology works for political groups as a whole, but is not very scalable to different domains.
        - Meaning if this were to be for micro-demographics
            - such as Hispanic Single Mother’s, Age 30-38, Income $35,000, …
        - How would you train it on a media diet that is specific to that demographic
    - This methodology only works for media sources and wouldn’t necessarily convert to say marketing a specific brand of cat food.

# Experiment Design/Methodology

## Modeling a Demographic

- To appropriately test this, we need to choose a population to model the demographic of
    
    ### Modeling the population of Montana
    
    - Collecting Accurate Population Data
        - Using the US Census data of Montana to create the sub demographic groups
            - Link: https://www.census.gov/quickfacts/MT
            - https://www.pewresearch.org/religion/religious-landscape-study/state/montana/party-affiliation/#social-and-political-views
            - [https://www.statista.com/statistics/1022697/montana-population-share-age-group/#:~:text=In 2021%2C about 12.7 percent,old in that same year](https://www.statista.com/statistics/1022697/montana-population-share-age-group/#:~:text=In%202021%2C%20about%2012.7%20percent,old%20in%20that%20same%20year).
            - https://ageconmt.com/part-2-rural-vs-urban-differences-common/
            - https://www.incomebyzipcode.com/montana
            - https://www.census.gov/library/stories/state-by-state/montana-population-change-between-census-decade.html
        - This data was then used to best distribute the demographics

### Building out personas

- A persona after stratifying off of general demographics is shown below

```json
{
        "Name": "Cassandra Taylor",
        "Age": "31",
        "Ethnicity": "White",
        "Gender": "Female",
        "Income": "53300",
        "Education Level": "High school graduate",
        "Political Affiliation": "Conservative",
        "Geographic Location": "Rural Montana, United States",
        "Veteran Status": "Is a Veteran",
}
```

- This while providing some context into a simulated person does not account for their beliefs and life experiences that could affect their views on certain issues
- The following fields were then generated using GPT-4 to attempt to provide more context for a model to act as the persona
    - Occupation, Media Consumption, Voting History, Group Membership, Influential Figures, Personal Anecdotes, Future Aspirations and Concerns
- This resulting in a very in depth persona
    
    ```json
    {
            "Name": "Cassandra Taylor",
            "Age": "31",
            "Ethnicity": "White",
            "Gender": "Female",
            "Income": "53300",
            "Education Level": "High school graduate",
            "Political Affiliation": "Conservative",
            "Geographic Location": "Rural Montana, United States",
            "Veteran Status": "Is a Veteran",
            "Occupation": "Veterinary Assistant.",
            "Media Consumption": "As a 31-year-old white female with a high school education and an annual income of $53,300 living in rural Montana, I tend to consume media outlets that align with my conservative political affiliation and geographic location. This includes Fox News, Newsmax, OANN, as well as local news outlets such as the Billings Gazette, the Missoulian, and the Bozeman Daily Chronicle. I also enjoy listening to conservative talk radio shows like Rush Limbaugh and Sean Hannity.\n\nBeing a veterinary assistant, I also enjoy consuming media related to animals and pets, such as Animal Planet and the AVMA website.\n\nDue to my busy schedule, I typically only have a few hours of free time each day to consume media, which is mostly in",
            "Voting History": "As an avid voter, I consistently support the Republican Party and their candidates due to their conservative values of limited government, individual freedom, and personal responsibility. Additionally, I align with their stance on important issues such as gun rights, pro-life, and lower taxes. Nevertheless, I ensure to thoroughly research each candidate and their policies before making my decision.",
            "Group Membership": "As a Montana-based veterinary assistant with conservative political beliefs, I am not affiliated with any political or social groups currently. Despite this, I stay updated on local and national politics and remain informed on issues that matter to me. In my profession, I am a member of the American Veterinary Medical Association (AVMA) and attend their conferences and events when feasible.",
            "Influential Figures": "As a conservative, I greatly respect Ronald Reagan for his leadership in upholding conservative values and restoring American pride and confidence. Similarly, I also admire Justice Clarence Thomas for his unwavering commitment to the Constitution and standing up for his beliefs, even in challenging situations.",
            "Personal Anecdotes": "As a veteran and someone who grew up in a rural area, my political views have been shaped by personal experiences. My service in the military has given me a deep appreciation for the sacrifices made by service members and their families, leading me to support policies that prioritize national security and military support. In addition, I believe in limited government and personal responsibility, having seen firsthand their importance in rural areas. Finally, working in the veterinary field has shown me the impact of government regulations on small businesses. Therefore, I support lower taxes and fewer regulations to help small businesses thrive and create jobs in our communities.",
            "Future Aspirations/Concerns": "As a conservative Montanan, I prioritize maintaining traditional values and preserving our way of life. I believe in prioritizing individual freedom, limited government, personal responsibility, supporting our military, and national security. However, I fear our country is becoming too divided and losing sight of what unites us. I also worry about government regulations, high taxes, and their impact on small businesses, which are the backbone of our communities. Lastly, I am concerned about the future of our healthcare system and ensuring quality care for all without bankrupting our country."
        }
    ```
    

### Polling

- Need to have a real poll taken of Montana Residents of around time of census to test the efficacy of this method.
    - Political Poll
        - https://dailymontanan.com/2022/10/26/montana-poll-shows-voters-unhappy-with-politicians-media-supreme-court-but-leaning-republican/
    - General Poll
        - https://crown-yellowstone.umt.edu/voter-surveys/2022/2022-u-of-montana-statewide-analysis-1.pdf
- Each persona was asked each question and prompting to given reasoning for how they thought through the question and an answer

## Results

### Current Issues with Polling

- Bias and censorship within Chat GPT
    - Chat GPT (mainly 3.5-turbo) with respond with “I am an AI model and thus have no opinions)
    - The most flagrant issue with gpt-3.5 and answering question was when asked:
        - “Do you believe trans-gender people should be able to change their gender on their birth certificate”
        - 100% of the personas said yes, while the real pole had around 55% of respondents saying no
            - A 70 year veteran in Montana, with many of his details be right wing, saying they support trans rights is highly unlikely
        - GPT-4 provided more realistic responses when tested on this question, but still was not on par with the true poll
    - Both GPT 3.5 and 4 when asked “Do you plan to vote?” always said yes
    - GPT-4 had better results with capturing the sentiment of a persona and answering with more consistent with target demographic

## Pricing

- Using GPT-3.5-turbo for making personas
    - Runtime: ~30 minutes
    - `1485993` tokens
    - Cost = $ `2.971986`
- Using GPT-3.5-Turbo for answering questions
    - Params
        - 343 Personas
        - 28 Questions
    - Runtime 3-4 hours
    - Tokens
        - Prompt = `12327371`
        - Completion = `1243197`
        - Total and Cost = `$ 27.141136`
- Estimated price of GPT-4
    - Using question batching, I have estimated to run GPT-4 would cost around $80
- This is much cheaper than a actual poll, which sources say costs 20-40 per response, so for a 1000 person, 30 question poll, you are looking at $80 instead of $20,000-$40,000, but that assumes the results you obtain are actually representative of the population.

# Conclusion

- There is much bias in the current GPT-3.5 model that comes out and can greatly affect the outcomes.  GPT-4 has a much better capability to provide meaningful results, but comes at a 20X price tag for token usage.
- For this methodology to be viable, extensive testing on the bias of a model needs to be done to ensure valid and representative results
- This being said, the ability for GPT-4 to provide salient answers shows that the technology could have some promise and if prompted better or with a more powerful and less biased model could provide meaningful results

# Future Work

- Running the simulation with GPT-4 to compare full results to GPT-3.5
- Better structuring of prompts and personas to get better feedback

[Prompt Change For Question Asking](https://www.notion.so/Prompt-Change-For-Question-Asking-1443e302b1f747e18050fd554e3feca6?pvs=21)
