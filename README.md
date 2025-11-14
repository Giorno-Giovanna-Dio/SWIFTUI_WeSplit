# WeSplit
credit to:
[hacking with swift](https://www.hackingwithswift.com/100/swiftui/16)
## App Guidelines:
### What is this app aim to solve?
- A app for check spliting.
### Who is this app for?
- Anyone who wants to share a check :)
### When is this app applicable?
- Having meals with friends or family, Buying things together... and a lot more ~
### How can user use this app? (Demo Video Below)
[![Demo Video](https://img.youtube.com/vi/7df22bI3hK8/0.jpg)](https://youtu.be/7df22bI3hK8)
---
## Code Implementation
```Swift
import SwiftUI

struct ContentView: View {
    @State private var checkAmount = 0.0
    @State private var numberOfPeople = 0
    @State private var tipPercentage = 20
    @FocusState private var amountIsFocused: Bool
    
    let tipPercentages = [10, 15, 20, 25, 0]
    
    var totalPerPerson: Double {
        let peopleCount = Double(numberOfPeople + 2)
        let tipSelection = Double(tipPercentage)

        let tipValue = checkAmount / 100 * tipSelection
        let grandTotal = checkAmount + tipValue
        let amountPerPerson = grandTotal / peopleCount

        return amountPerPerson
    }
    
    var body: some View {
        NavigationStack{
            Form {
                //check input
                Section{
                    TextField("Amount", value: $checkAmount, format: .currency(code: Locale.current.currency?.identifier ?? "NTD"))
                        .keyboardType(.decimalPad)
                        .focused($amountIsFocused)
                    Picker("Number of people", selection: $numberOfPeople) {
                            ForEach(2..<100) {
                                Text("\($0) people")
                            }
                        }.pickerStyle(.navigationLink)
                }
                //tip input
                Section ("How much tip do you want to leave?"){
                    Picker("Tip percentage", selection: $tipPercentage) {
                        ForEach(tipPercentages, id: \.self) {
                            Text($0, format: .percent)
                        }
                    }.pickerStyle(.segmented)
                }
                //check plus tips
                Section("Total including tip") {
                    Text(checkAmount + (checkAmount / 100 * Double(tipPercentage)), format: .currency(code: Locale.current.currency?.identifier ?? "NTD"))
                }
                
                //result
                Section("Amount per person") {
                        Text(totalPerPerson, format: .currency(code: Locale.current.currency?.identifier ?? "NTD"))
                    }
            }.navigationTitle("WeSplit")
                .toolbar {
                    if amountIsFocused {
                        Button("Done") {
                            amountIsFocused = false
                        }
                    }
                }
        }
    }
}

#Preview {
    ContentView()
}

```
