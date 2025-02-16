<%_*
// OpenAI API 키 설정
const OPENAI_API_KEY=""_%>

<%_*
// 프롬프트 정의 - 토픽 생성을 위한 프롬프트
const topics_prompt = `You are an expert in generating an appropriate topics properties that will be used in Obsidian Note. Your mission is to generate one or more topics suitable for given content.
Your generated output must be comma-separated values.

### Example Output:

[[📚 214 Document Parser]], [[📖 200 AI & Data]], [[📚 601 Enterprise Outsourcing Projects]]

Here's a list of possible substitutions for topics.
You must use these topics listed below:

### Topics:
- [[📖 100 연구]]
	- [[📚 101 RAG 연구]]
	- [[📚 102 LLM 연구]]
	- [[📚 103 AI 트렌드]]
	- [[📚 104 AI 활용 사례]]
	- [[📚 105 최신 논문]]

- [[📖 200 AI & 데이터]]
	- [[📚 201 Concepts]]
	- [[📚 202 Prompt]]
	- [[📚 203 Retriever]]
	- [[📚 204 Embedding]]
	- [[📚 205 LLM]]
	- [[📚 206 LocalModels / Ollama]]
	- [[📚 207 Vector DB]]
	- [[📚 208 Reranking]]
	- [[📚 209 Knowledge Graph]]
	- [[📚 210 Hybrid Search]]
	- [[📚 211 Evaluation]]
	- [[📚 212 HuggingFace]]
	- [[📚 213 Agent]]
	- [[📚 214 문서파서]]
	- [[📚 220 Fine-Tuning]]
	- [[📚 221 LangChain / LangGraph]]
	- [[📚 222 vLLM]]
	- [[📚 230 데이터 분석]]
	- [[📚 231 머신러닝]]
	- [[📚 232 딥러닝 (pytorch)]]
	- [[📚 233 데이터 엔지니어링]]
	- [[📚 250 AI 서비스 구축]]

- [[📖 300 개발]]
	- [[📚 301 Python]]
	- [[📚 302 Web 개발]]
	- [[📚 303 AWS]]
	- [[📚 304 Docker & Kubernetes]]
	- [[📚 305 시스템 설계]]
	- [[📚 306 MLOps & 배포]]
	- [[📚 307 Git]]
	- [[📚 308 ngrok]]
	- [[📚 309 FastAPI]]
	- [[📚 310 가상환경/패키지관리(PYPI, Poetry, UV)]]

- [[📖 400 비즈니스 & 운영]]
	- [[📚 401 Braincrew 운영]]
	- [[📚 402 회사]]
	- [[📚 403 HR]]
	- [[📚 404 업무용 구매]]

- [[📖 500 강의 & 컨설팅]]
	- [[📚 501 기업 컨설팅]]
	- [[📚 502 강의 자료]]
	- [[📚 503 교육 커리큘럼]]
	- [[📚 504 세미나 & 워크숍]]

- [[📖 600 프로젝트]]
	- [[📚 601 기업 외주 프로젝트]]
	- [[📚 602 사이드 프로젝트]]

- [[📖 700 콘텐츠 & SNS]]
	- [[📚 701 YouTube 테디노트]]
	- [[📚 702 SNS 운영]]

- [[📖 800 지식관리 & 생산성]]
	- [[📚 801 세컨드브레인]]
	- [[📚 802 메모 & 노트]]
	- [[📚 803 나의 생각 정리]]
	- [[📚 804 업무 자동화]]

- [[📖 900 Personal & 성장]]
	- [[📚 901 커리어]]
	- [[📚 902 네트워킹 & 커뮤니티]]
	- [[📚 903 개인 구매]]

####

Example Output:

[[📚 214 Document Parser]], [[📖 200 AI & Data]], [[📚 601 Enterprise Outsourcing Projects]]

####

[Note] Write your Final Answer in Korean. DO NOT narrate, just write the output without any markdown formatting of wrapping.
Generated topics must be related to the content. Otherwise, you will be penalized.
`
_%>
 
<%_*
// frontmatter의 topics 속성을 업데이트하는 함수
// @param file: 대상 파일 객체
// @param newTopics: 새로운 토픽 문자열 (쉼표로 구분된 값)
const processTopics = async (file, newTopics) => {
  await tp.app.fileManager.processFrontMatter(file, (frontmatter) => {
    // 쉼표로 구분된 토픽을 배열로 변환하고 각 항목의 앞뒤 공백 제거
    const topics = newTopics.split(',').map(topic => topic.trim());
    // frontmatter의 topics 속성 업데이트
    frontmatter.topics = topics.map(topic => `${topic}`);
  });
};
_%>

<%_*
// 현재 노트의 내용을 가져옵니다
const fileContent = tp.file.content;

// OpenAI API를 호출하여 토픽을 생성하는 함수
async function generateTopics(content) {
    try {
        const response = await tp.obsidian.requestUrl({
            method: "POST",
            url: "https://api.openai.com/v1/chat/completions",
            contentType: "application/json",
            headers: {
                "Authorization": `Bearer ${OPENAI_API_KEY}`,
            },
            body: JSON.stringify({
                model: "gpt-4o",
                messages: [
                    { "role": "system", "content": topics_prompt },
                    { "role": "user", "content": `Here is the content of the note:\n${content}` }
                ]
            })
        });
        
        return response.json.choices[0].message.content;
    } catch (error) {
        console.error('토픽 생성 중 오류 발생:', error);
        return '토픽 생성 실패';
    }
}

// 토픽을 생성하고 파일에 적용
const topics = await generateTopics(fileContent);
const file = tp.config.target_file;
await processTopics(file, topics);
_%>
