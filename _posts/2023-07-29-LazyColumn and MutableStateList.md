---
layout: post
author: Yasu
tags: [Android, Jetpack Compose, State]
---
I have a list of borrowed books to show on the screen. When the title of a book is clicked, a dialog pops up and you can confirm the return of that book. What I want to do then is letting the title of the book disappear from the list.

Here is the process to do so:
1. Click the return button which is found in a column of the LazyColumn
2. It fires onClickBorrowedBook which remove the clicked book's title from the borrowedBooksList
3. As borrowedBooksList is a snapshotStateList and thus observable, the step 2 triggers a recomposition of the ReturnScreen.

```
@Composable
fun ReturnScreen(
    borrowerState: State<BookBorrower>,
) {
    val borrowedBooksList = remember {
        borrowerState.value.borrowedBooksList.toMutableStateList()
    }
  /*Or, you can initialise this way:
  val borrowedBooksList2 = remember {
      mutableStateListOf<BorrowedBook>().apply { 
          addAll(borrowerState.value.borrowedBooksList)
      }
  }
  */
    
    LazyColumn {
	  items(
		items = borrowedBooksList,
		itemContent = { borrowedBook ->
			BorrowedBookListItem(
				borrowedBook = borrowedBook,
				onClickBorrowedBook = {
					val bookTitleToErase = borrowedBook.title
					onClickBorrowedBook(bookTitleToErase)
					borrowedBooksList.remove(borrowedBook)
				}
			)
		}
	  )
    }
}
```