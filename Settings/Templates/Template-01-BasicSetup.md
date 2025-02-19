<%_*
// 노트 내용
const fileContent = tp.file.content;
_%>

<%_*
const noteType = await tp.system.suggester(
    ["[[📌 개발노트]]", "[[📌 미팅노트]]", "[[📌 강의노트]]", "[[📌 스크래치노트]]"], 
    ["[[📌 개발노트]]", "[[📌 미팅노트]]", "[[📌 강의노트]]", "[[📌 스크래치노트]]"], 
    true, 
    "노트 옵션을 선택하세요.");

console.log("noteType: ", noteType);

// file 가져오기
const file = tp.config.target_file

await tp.app.fileManager.processFrontMatter(file, (frontmatter) => {
  // metadata 업데이트
  frontmatter.created = tp.file.creation_date();
  frontmatter.author = "mellowresearch";
  frontmatter.source = "";
  frontmatter.type = noteType;
});
_%>
