<%*
const fileName = tp.file.title; // Get the filename (e.g., "2025-W12")

const match = fileName.match(/(\d{4})-W(\d{1,2})/);

if (match) {

    const year = parseInt(match[1], 10);
    const week = parseInt(match[2], 10);

    // Function to get the start of a given ISO week
    function getStartOfISOWeek(year, week) {

        let firstThursday = new Date(year, 0, 4); // Jan 4 is always in week 1

        let firstMonday = new Date(firstThursday.setDate(firstThursday.getDate() - (firstThursday.getDay() + 6) % 7));

        return new Date(firstMonday.setDate(firstMonday.getDate() + (week - 1) * 7));

    }

  

    const startOfWeek = getStartOfISOWeek(year, week);

    let output = "";

    for (let i = 0; i < 7; i++) {

        let date = new Date(startOfWeek);

        date.setDate(startOfWeek.getDate() + i);

        let formattedDate = date.toISOString().split("T")[0];
	    let dailyNotePath = `Periodic Notes/Daily Notes/${formattedDate}.md`; // Adjust folder if needed

	    output += `## ${formattedDate}\n![[${dailyNotePath}]]\n\n`;

	}

	tR += output;
}

%>

