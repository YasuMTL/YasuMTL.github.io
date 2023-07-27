---
layout: post
author: Yasu
tags: [Android, Jetpack Compose, Dialog]
---

Here is my very first post on this blog, which I created with Jekyll and Github Pages a few weeks ago, expecting to share my commitment on the development of an Android app. Unfortunately, since I began to have a temporary job at the end of this April, my time and energy have been fairly limited. I'll do my best to keep posting on a regular basis.

For several months, I have been developing a barcode scanner app which would make it easy to borrow and return books in the library of a Japanese school in Montréal. And today I have implemented a dialog which pops up when one of the books name clicked in the list. The dialog is to confirm the return of the book. At first, I tried to do this using a class which extends DialogFragment, but I came across some recommendation of using [Dialog Composable](https://developer.android.com/reference/kotlin/androidx/compose/ui/window/package-summary#Dialog(kotlin.Function0,androidx.compose.ui.window.DialogProperties,kotlin.Function0)). It took me some time to understand how to call and hide the dialog, but I eventually made it as shown below: 

```
@Composable
fun BorrowedBookListItem(
    borrowedBook: BorrowedBook,
    onClickBorrowedBook: () -> Unit = {}
) {
    val returnBook = remember { mutableStateOf(false) }
    val callDialog = remember { mutableStateOf(false) }
    
    if (callDialog.value) {
        CustomDialog(
            openDialogCustom = callDialog,
            returnBook = returnBook
        )
    }

    if (returnBook.value) {
        Log.d("ReturnScreen", "User wishes to return book!")
        onClickBorrowedBook()
        returnBook.value = false
    }

    Row (verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.Center,
        modifier = Modifier
            .fillMaxWidth()
            .fillMaxHeight()
    ){
        Column (
            modifier = Modifier.weight(3F),
            horizontalAlignment = Alignment.CenterHorizontally
        ){
            Text(
                text = borrowedBook.title,
                textAlign = TextAlign.Center,
                fontWeight = FontWeight.Bold
            )
            Text(text = borrowedBook.dateToReturn)
        }
        Button(
            modifier = Modifier
                .height(50.dp)
                .padding(5.dp)
                .weight(2F),
            onClick = { callDialog.value = true }
        ) {
            Text(text = "返却する")
        }
    }
}

@Composable
fun CustomDialog(
    openDialogCustom: MutableState<Boolean>,
    returnBook: MutableState<Boolean>
) {
    Dialog(onDismissRequest = {
        openDialogCustom.value = false
    }) {
        ReturnConfirmationDialogUI(
            openDialogCustom = openDialogCustom,
            returnBook = returnBook
        )
    }
}

@Composable
fun ReturnConfirmationDialogUI(
    modifier: Modifier = Modifier,
    openDialogCustom: MutableState<Boolean>,
    returnBook: MutableState<Boolean>
){
    Card(
        shape = RoundedCornerShape(10.dp),        
        modifier = Modifier.padding(10.dp,5.dp,10.dp,10.dp),
        elevation = 8.dp
    ) {
        Column(
            modifier
                .background(Color.White)
		){
			Column(modifier = Modifier.padding(16.dp)) {
				Text(
					text = "この本を返却しても\nよろしいですか？",
					textAlign = TextAlign.Center,
					modifier = Modifier
						.padding(top = 5.dp)
						.fillMaxWidth(),
					style = MaterialTheme.typography.h5,
					maxLines = 2,
					overflow = TextOverflow.Ellipsis
				)
				Row(
					Modifier
						.fillMaxWidth()
						.padding(top = 10.dp)
						.background(Color.LightGray),
					horizontalArrangement = Arrangement.SpaceAround
				) {
					TextButton(onClick = {
						openDialogCustom.value = false
						returnBook.value = false
					}) {
						Text(
							"いいえ",
							fontWeight = FontWeight.Bold,
							color = Color.Gray,
							modifier = Modifier.padding(top = 5.dp, bottom = 5.dp)
						)
					}
					TextButton(onClick = {
						openDialogCustom.value = false
						returnBook.value = true
					}) {
						Text(
							"はい",
							fontWeight = FontWeight.ExtraBold,
							color = Color.Black,
							modifier = Modifier.padding(top = 5.dp, bottom = 5.dp)
						)
					}
				}
			}
		}
	}
}
```