---
title: "Split multilined cells into multiple rows"
layout: page
tag: Modus Operandi
---

If you found this page while trying to figure this out perhaps you've made the same mistake that I did. If you want to scrape tables from websites, I'd recommend using excel's "Import data from the web" feature. Yes, I know it's proprietary software, but if you're trying to do a lot of things really quickly this would probably save the most amount of time.

If you're comfortable enough with Pandas, you could try the following task for yourself. But be warned, `explode()` is very confusing. I malded at it for like three hours and I still don't understand what went wrong.

Anyways, let me show you this dirty trick I discovered during the middle of a hackathon in august.

This is a [CSV](/assets/media/vi2.csv) I generated from a [page](https://www.canada.ca/en/health-canada/services/food-nutrition/healthy-eating/dietary-reference-intakes/tables/reference-values-vitamins-dietary-reference-intakes-tables-2005.html) on Statistics Canada which I needed for my [project](https://github.com/echometerain/canada-meal-planner). I did not have much experience scraping data from the internet so my first instinct was to copy the `<table>` section of the source code into some random website that claims to [convert html tables to CSVs](https://www.convertcsv.com/html-table-to-csv.htm). I ended up with a spreadsheet that looked like this:

<img src="/assets/media/horror.png" alt="Spreadsheet Image" title="Software: LibreOffice Calc" width="100%">

There are newline characters where cell separators are supposed to be. Should be simple to fix, right?

Unfortunately, no. Splitting cells like this is supported neither in Excel nor in its derivatives like LibreOffice Calc, and figuring out how to do it programmatically (without using pandas) seemed like another hackathon project within itself.

What I decided on in the end was to use a find-and-replace operation within a text editor. However, that could not be done in this spreadsheet's current form. CSVs store data in rows, which means that I would need to move multiple values over multiple lines while those line numbers are constantly changing.

<img src="/assets/media/unstable.png" alt="Values move long distances" width="100%">

So I had to transpose it, and the following is a list of instructions for fixing the all cells:

- Go into your spreadsheet software
- Select all the cells in the spreadsheet with data (and only the ones with data)
- Copy the content to your clipboard
- Delete the cells
- Right click the top-left cell of the range you just deleted and select `Paste special->Transpose`
- Find-and-replace all occurrences of the newline character "\n" with a sequence of characters that's not used anywhere else in your spreadsheet.
- Copy the transposed content to your clipboard
- Delete the cells again
- Paste it into a text editor
- Notice how the horizontal separator is now encoded as a tab character ("\t")
- Replace all occurrences of the sequence of characters your chose with "\t"
- Copy it and `Paste special->Transpose` it back into your spreadsheet software, and voila!
