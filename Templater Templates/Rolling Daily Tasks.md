<%*
const currentDateStr = tp.file.title;
const currentDate = new Date(currentDateStr);

// Format current and previous dates as "YYYY-MM-DD"
const formattedDate = currentDate.toISOString().split("T")[0];
const prevDate = new Date(currentDate);
prevDate.setDate(prevDate.getDate() - 1);
const prevFormattedDate = prevDate.toISOString().split("T")[0];

const prevFile = tp.file.find_tfile(prevFormattedDate);
if (!prevFile) {
	tR += "No previous note found.";
}else{
	const prevContent = await app.vault.read(prevFile);
	const taskRegex = /^- \[ \] .+/gm;
	const originalTasks = prevContent.match(taskRegex);
	if (originalTasks && originalTasks.length > 0) {
		// Replace all "- [ ]" with "- [>]" in the previous file
		const updatedText = prevContent.replace(/^- \[ \]/gm, '- [>]');
		await app.vault.modify(prevFile, updatedText);
		// Append the original tasks to the current note
		tR += `## Rolled Over Tasks\n`;
		tR += originalTasks.join("\n") + "\n";
	}
}
%>

