
# Guide : Keyboard 000 Implementation Android
> Credits @aldianrahman @andi @sani @fikri

> Doc by @sani @fikri

## Code Only Implement
1. Cari kotlin code untuk UI yang ingin di implementasikan,
2. Terapkan code berikut di `init` atau `onCreate()` atau `onSuccess()` intinya ketika InputField muncul
```kotlin
val keyboardView = BaleKeyboard(this@SimulationDetailView)  
keyboardView.setupKeyboard(
	listOf(
		inputPropView.getEditText() to 13, 	
		inputUangMuka.getEditText() to 13),
	KEYBOARD_TYPE.TRIPLE_ZERO) 
binding.root.addView(keyboardView)
```
contoh
```kotlin
val inputPropView: InputViewRevamp = simulationKPRView?.findViewById(R.id.inputPropertyPrice)!!  
val inputUangMuka : InputViewRevamp = simulationKPRView?.findViewById(R.id.inputDownPayment)!!  
val inputBunga : InputView2Revamp = simulationKPRView?.findViewById(R.id.inputInterestRatePerYear)!!  
val keyboardView = BaleKeyboard(this@SimulationDetailView)  
val keyboard2View = BaleKeyboard(this@SimulationDetailView)  
keyboardView.setupKeyboard(listOf(inputPropView.getEditText() to 13, inputUangMuka.getEditText() to 13),KEYBOARD_TYPE.TRIPLE_ZERO)  
keyboard2View.setupKeyboard(listOf(inputBunga.getEditText() to null), type = KEYBOARD_TYPE.DOT)
```
3. Variable `inputPropView` adalah `InputField` yang ingin di munculkan keyboardnya
4. Variable `limit` adalah limit digitnya
5. Variable `KEYBOARD_TYPE.TRIPLE_ZERO` or `KEYBOARD_TYPE.COMA` or `KEYBOARD_TYPE.DOT`
7. Cari code `onResume()` lalu tambahkan code berikut untuk clear fokus
```kotlin
override fun onResume() {  
    super.onResume()  
    binding.inputNominal.clearFocus()  // code ini
}
```

## Keyboard COMMA/DOT
> `KEYBOARD_TYPE.COMA` -> , 
> `KEYBOARD_TYPE.DOT` -> .

## Custom adjustment (if necessary)
1. Siapkan inputView yang mau ditambahkan keyboard comma, tambahkan type ini di 

`enum class` -> `REVAMP_CURRENCY_DECIMAL`

**onTextChanged**
```kotlin
TYPE.REVAMP_CURRENCY_DECIMAL -> {  
    s?.toString()?.let {  
  binding.txtInput.removeTextChangedListener(this)  
        if (it.startsWith(",")) {  
            it.trimStart(',')  
        }  
        binding.txtInput.addTextChangedListener(this)  
    }  
}
```
**afterTextChanged**
```kotlin
TYPE.REVAMP_CURRENCY_DECIMAL -> {  
    if (s.toString() != currentCurrency) {  
        binding.txtInput.removeTextChangedListener(this)  
  
        val cleanString = s.toString().let { char ->  
  if (char.count { it == ',' } > 1 && char.last() == ',') return@let char.dropLast(  
                1  
  )  
            return@let char  
  }.replace(".", "")  
        var formatted = ""  
  if (cleanString.isNotEmpty()) {  
  
            formatted = cleanString.replace(".", ",")  
            if (formatted.contains(',') && formatted.length > 1) {  
                if (formatted.first() == ',') {  
                    formatted = ""  
  } else {  
                    val listFormatted = formatted.split(',')  
                    val thousand = listFormatted[0].toDouble().currencyFormatter()  
                    val comma =  
  "," + listFormatted[1].let {  
  if (it.length > 2) it.dropLast(  
                                1  
  ) else it  
  }  
  formatted = thousand + comma  
  }  
            } else {  
                formatted =  
 if (cleanString == ",") cleanString.replace(",", "").toDouble()  
                        .currencyFormatter() else cleanString.toDouble()  
                        .currencyFormatter()  
            }  
            currentCurrency = formatted  
  } else {  
            currentCurrency = ""  
  }  
  
        binding.txtInput.setText(currentCurrency)  
        binding.txtInput.setSelection(currentCurrency.length)  
  
        binding.txtInput.addTextChangedListener(this)  
    }  
}
```
**setInputType**
```kotlin
TYPE.REVAMP_CURRENCY_DECIMAL -> {  
    InputType.TYPE_NUMBER_FLAG_DECIMAL  
}
```



## Steps
1. Siapkan Screen yang ingin di implementasikan, misal **Pindah Dana** cari *activity* dan cari layout xml nya yaitu *activity_pindah_dana.xml*
2. Paste code berikut di paling bawah layout diatas 
`</androidx.constraintlayout.widget.ConstraintLayout>`
```xml
		<net.ist.btn.shared.ui.BaleKeyboard  
		  android:id="@+id/keyboard_view"  
		  android:layout_width="match_parent"  
		  android:layout_height="wrap_content"  
		  android:visibility="gone"  
		  app:layout_constraintBottom_toBottomOf="parent"  
		  />
``` 
menjadi 
```xml
		<net.ist.btn.shared.ui.BaleKeyboard  
		  android:id="@+id/keyboard_view"  
		  android:layout_width="match_parent"  
		  android:layout_height="wrap_content"  
		  android:visibility="gone"  
		  app:layout_constraintBottom_toBottomOf="parent"  
		  />
</androidx.constraintlayout.widget.ConstraintLayout>
``` 
3.  Kembali ke activity **Pindah Dana** cari InputField yang ingin di tambahkan keyboardnya misal `binding.inputNominal`, set `limit` dengan batas limit, `setInputType` ke custom/null
```kotlin
binding.inputNominal.setInputType(InputNominalTransferView.TYPE.KEYBOARD_CUSTOM)
```
untuk input field dengan jenis lain gunakan type berikut
```kotlin
// InputType.TYPE_NULL
binding.inputNominal.setInputType(InputNominalTransferView.TYPE.TYPE_NULL)
```
lalu setup keyboard seperti ini
```kotlin
binding.keyboardView.apply {  
  setEditText(binding.inputNominal.getEditText(),KEYBOARD_TYPE.TRIPLE_ZERO)  
  binding.inputNominal.getEditText().afterTextChanged {  
  if (it.length > limit){  
            binding.keyboardView.disableTripleNol()  
        }else{  
            binding.keyboardView.enableTripleNol()  
        }  
    }  
}
```
4. Cari code `onResume()` lalu tambahkan code berikut untuk clear fokus
```kotlin
override fun onResume() {  
    super.onResume()  
    binding.inputNominal.clearFocus()  // code ini
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY0NzQwMTQwNF19
-->