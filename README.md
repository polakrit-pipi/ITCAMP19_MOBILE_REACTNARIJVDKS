
import { React ,  useState } from 'react'
import { View, Text, StyleSheet, TextInput, Button, ScrollView , TouchableOpacity } from 'react-native'
import TodoItem from '../components/ToDoItem'
import Dialog from "react-native-dialog"

function HomeScreen({ navigation }) {
  const [items, setItems] = useState([])
  const [name, setName] = useState("")
  const  [content , setContent] = useState("")
  
  const [selectedTodoId , setSelectedTodoId] = useState("")
  const [selectedToDoName , setSelectedTodoName] = useState("")
  const [selectedToDoContent , setSelectedToDoContent] = useState("")
  const [showDetailDialog , setShowDetailDialog] = useState(false)

  const getNewToDoId = () => {
    const newToDoId = Math.floor(Math.random(111111 , 999999) * 10000 ) // 111111 - 999999
    while (true) {
      if (items.some((item) => item.id === newToDoId)){
        newToDoId = Math.floor(Math.random(111111,999999) * 10000)

      }
      else{
        break
      }

    }
    return newToDoId
  }
  const handleAddNewItem = () => {
    const newItem = {
      id: getNewToDoId(),
      name: name,
      content: content,
      createAt: new Date().toLocaleString()
    }

    //[1,2,3]

    setItems([newItem, ...items])
    setName("")
    setContent("")
  }

  const ToDoDetailDialog = () => {
    const [newName , setNewName] = useState("")
    const [newContent , setNewContent] = useState("")

    const handleUpdate  = () =>{
      //cosnt nums  = [2,1,3]
      // nums[1] =  4 / num = [2,4,3] 
      const targetIndex = items.findIndex((item) =>  item.id === selectedTodoId)
      
      let tempItems = [...items]
      let targetItem = {...tempItems[targetIndex]}
      targetItem.name = newName || selectedToDoName
      targetItem.content = newContent || selectedToDoContent

      tempItems[targetIndex] = targetItem
      setItems(tempItems)
      setShowDetailDialog(false)
    }

    const handleDelete = () => {
      const updateToDos = items.filter((item) => item.id !== selectedTodoId)
      setItems(updatedToDos)
      setShowDetailDialog(false)
    }
    return(
      <Dialog.Container visible={showDetailDialog}>
        <Dialog.title>
          My ToDo Details     
       </Dialog.title>

          <Dialog.Description>Update or Delete this todo</Dialog.Description>
       <Dialog.Input placeholder= {selectedToDoName} onChange = {setNewName}/>
       <Dialog.Input placeholder= {selectedToDoContent} onChange = {setNewContent}/>
        <Dialog.Button label="cancle" onPress={() => setShowDetailDialog(false)} />
        <Dialog.Button label="Update" onPress={handleUpdate} />
        <Dialog.Button label="Delete" onPress={handleDelete} />

      </Dialog.Container>
    )
  }
  return (
    <ScrollView style={{ padding: 18 }}>
      <ToDoDetailDialog />
      <View>
        <Text style={styles.headerTitle}>ITCamp To-Do Lists</Text>

        <View style={{ borderRadius: 6, borderWidth: 1, marginTop: 12 }}>
          <Text style={styles.inputLabel}>Title</Text>
          <TextInput
            style={styles.textInput}
            onChangeText={setName}
            value={name}
          />

          <Text style={styles.inputLabel}>Content</Text>
          <TextInput
            style={{ ...styles.textInput, height: 100 }}
            onChangeText={setContent}
            value={content}
            multiline={true}
            numberOfLines={6}
          />

          <View style={{ padding: 12 }}>
            <Button
              title='Create a new to-do'
              onPress={handleAddNewItem}
            />
          </View>
        </View>
        { items.map(({id , name ,content , createAt}) => {
          return (
            <TouchableOpacity key = {id} onPress= {() => {
              setSelectedTodoId(id)
              setSelectedTodoName(name)
              setSelectedToDoContent(content)
              setShowDetailDialog(true)
            }}>
              <TodoItem id={id} name = {name} content={content} createAt={createAt}/>

            </TouchableOpacity>
            // const nums = [1,2,3]
          )
        })}
      </View>
    </ScrollView>
  )
}

const styles = StyleSheet.create({
  textInput: {
    height: 40,
    margin: 12,
    borderWidth: 1,
    padding: 10,
  },
  headerTitle: {
    textAlign: "center",
    fontSize: 18,
    fontWeight: "bold",
    marginBottom: 6,
  },
  inputLabel: {
    padding: 12,
    paddingBottom: 0,
  },
});

export default HomeScreen
