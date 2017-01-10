# R Diary

Monday, January 9, 2017

I've used R informally since 2010, but would like to be a lot more fluent. I can usually manage to do the things I want to do in R, but 
I still find myself taking a lot longer than I want to to do relatively simple things (and usually doing them less elegantly than I would like).

I recently started using [Data Camp](www.datacamp.com) to learn R more systematically and expand my capabilities. This "R Diary" is for me
to keep track of my progress, take notes on some things I've learned, and celebrate being able to do things with less headache!

I plan on working through two Data Camp courses per month. I've completed [the introductory](https://campus.datacamp.com/courses/free-introduction-to-r) and [intermediate](https://www.datacamp.com/courses/intermediate-r) courses. And am currently working on the [intermediate practice](https://www.datacamp.com/courses/intermediate-r-practice) and [writing functions](https://www.datacamp.com/courses/writing-functions-in-r) courses. I'll be posting snipets of things I've learned in this diary.

Tuesday, January 10, 2017

A quick pedagogical note on something I find frustrating with the [Data Camp](www.datacamp.com) coursework I've done so far. 

The Data Camp grading mechanism often rejects as "incorrect" solutions which are not only correct, but are also simply better (e.g. more generalized). For example, in the [intermediate practice](https://www.datacamp.com/courses/intermediate-r-practice) course, the learner is prompted to write a function to extract information from a list. 

> "As the plant's data scientist, you'll often want to extract information from the list, be it the messages, the timestamp, whether or not it was a success, etc. You could write a dedicated for loop for each one of them, but this is rather tedious. Why not wrap it in a function?"

Starting with the skeleton code provided, I wrote a function that took an argument for the variable to be extracted (`var`) from the data (`list`). I then used that function to extract one of the variables (`timestamp`) from the data structure, and converted the extracted times back to `as.POSIXct` objects.

```
extract_info <- function(list, var) {
  info <- c()
  for (i in list) info <- c(info, i[var])
  return(unname(unlist(info)))
}

# Call extract_info() on logs
as.POSIXct(extract_info(logs, "timestamp"), origin = "1970-01-01")
```
Data Camp marked this as incorrect. What they wanted was a function that took in a data structure and extracted timestamps only. They also didn't require the learner to convert the extracted timestamps into a human-readable date format, accepting a vector of integers as a correct solution.

```
extract_info <- function(x) {
  info <- c()
  for (log in x) info <- c(info, log$timestamp)
  return(info)
}

extract_info(logs)
```

It turns out that generalizing the function was the very next exercise, but given that this course comes *after* a course where the learner has already been told about the importance of writing generalizable functions, it seems inane to penalize the learner for writing objectively better code. It may be difficult to integrate into the grading system, but I would suggest linking exercises such that a correct solution for the later exercise (the more generalized function) is also graded as "correct" for the earlier exercise (where `timestamp` is hard-coded as the input variable. The learner could then be given a feedback message congratulating them for using good coding practices and telling them that they will be skipping the next exercise. 
