# Question bank

Ask the questions whose answers would change the design. Never ask more than five at once. Skip questions already answered in the user's state files or ADRs.

## Contents
- Requirements and consequences
- Failure and recovery
- Data and sources
- Load and growth
- Delivery and consumers
- Operations and people
- Tool choices

## Requirements and consequences
- Who uses this result, and for what decision?
- What happens if it is a day late? If it is wrong and nobody notices?
- Is there a deadline in the day by which it must be ready?

## Failure and recovery
- What happens if it fails halfway through?
- How would you re-run last Tuesday only?
- How will you find out it failed, and how fast?
- If the source is unavailable for a day, what should happen?

## Data and sources
- Which system owns this data, and who can change its structure?
- How do you detect records that were updated or cancelled in the source?
- Can corrections arrive late? How late?
- What is the business key, and is it really unique?

## Load and growth
- How many rows and MB per run today, and in a year?
- How long does it take now, and what is the acceptable maximum?
- What is the peak memory used by the heaviest task?

## Delivery and consumers
- Does this need to be a file, or could consumers read from the datamart?
- How do you confirm the recipient actually got it?
- Does any other report calculate the same number?

## Operations and people
- If you were away for two weeks, who would run this, and with what document?
- How is this deployed, and how is it rolled back?
- What does this add to what you must monitor and upgrade?

## Tool choices
- Which measured problem does the new tool solve that the current stack cannot?
- What is the simplest version with existing tools, and why is it not enough?
- How would you remove this tool in a year if it does not work out?
