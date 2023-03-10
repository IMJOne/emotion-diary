![diary](https://user-images.githubusercontent.com/110226567/215329946-b0b7878b-a051-4f99-bbc1-86f792702fad.png)

# π Emotion diary

λλ§μ μμ κ°μ  μΌκΈ°μ₯ π [Demo](https://emotion-diary-jone.web.app/)

<br />

## π’ νλ‘μ νΈ κ°μ

μ€λμ κ°μ κ³Ό ν¨κ» κ°λ¨ν μΌκΈ°λ₯Ό μμ±ν  μ μλ κ°μ  μΌκΈ°μ₯μλλ€.<br />
μΌκΈ°λ μ λ¨μλ‘ κΈ°λ‘λλ©°, μνλ κΈ°μ€μ λ°λΌ λͺ©λ‘μ μ λ ¬ν  μ μμ΅λλ€.<br />
κΈ°μ‘΄μ ν¬λ λ¦¬μ€νΈλ³΄λ€λ μ‘°κΈ λ μ°Έμ ν νλ‘μ νΈλ₯Ό μ§ννκ³  μΆμ΄ μ μνκ² λμμ΅λλ€.

<br />

## π¨οΈ μ¬μ© κΈ°μ 

<p>
  <img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=React&logoColor=black"/>
  <img src="https://img.shields.io/badge/React Router-CA4245?style=flat-square&logo=React-Router&logoColor=white"/>
  <img src="https://img.shields.io/badge/PostCSS-DD3A0A?style=flat-square&logo=PostCSS&logoColor=white"/>
  <img src="https://img.shields.io/badge/Firebase-FFCA28?style=flat-square&logo=Firebase&logoColor=white"/>
</p>

<br />

## π μ£Όμ κΈ°λ₯

- μλ‘μ΄ μΌκΈ° μμ±
- μΌκΈ° μμ  λ° μ­μ 
- μ΅μ μ / μ€λλ μ μ λ ¬
- μ’μ κ°μ λ§ / λμ κ°μ λ§ μ λ ¬
- μΌκΈ° λͺ©λ‘ μ λ¨μλ‘ μ΄λ κ°λ₯

<br />

## π» μμ€ μ½λ

μ μ²΄ μ½λ λ³΄λ¬ κ°κΈ° π [Notion](https://imjone.notion.site/Emotion-diary-8a01a0f8e2fd43e2b84576eb631f6fb2)

### π λ¦¬λμ λ° λμ€ν¨μΉ ν¨μ μ μ

λ¨Όμ  μΌκΈ° μΆκ°/μμ /μ­μ  κΈ°λ₯μ λ΄λΉν  λ¦¬λμμ λμ€ν¨μΉ ν¨μλ₯Ό μ μν©λλ€.<br />
`onCreate`, `onRemove`, `onEdit`μ ν΅ν΄ νμν μ λ³΄λ₯Ό μΈμλ‘ λ£μ΄μ λμ€ν¨μΉ ν¨μλ₯Ό νΈμΆνλ©΄,<br />
λ¦¬λμ λ΄λΆμμλ κ° μΌμ΄μ€λ§λ€ μ λ¬ λ°μ `action.type`μ λ°λΌ μ μ νκ² μλ‘μ΄ μνλ₯Ό λ°νν©λλ€.

```javascript
// reducer
const [diary, dispatch] = useReducer(reducer, []);

const reducer = (diary, action) => {
  let newDiary = [];
  switch (action.type) {
    case 'INIT': {
      return action.data;
    }
    case 'CREATE': {
      newDiary = [action.data, ...diary];
      break;
    }
    case 'REMOVE': {
      newDiary = diary.filter(item => item.id !== action.targetId);
      break;
    }
    case 'EDIT': {
      newDiary = diary.map(item => (item.id === action.data.id ? { ...action.data } : item));
      break;
    }
    default:
      return diary;
  }
  return newDiary;
};
```

```javascript
// dispatch
const onCreate = (date, content, emotion) => {
  dispatch({
    type: 'CREATE',
    data: {
      id: uuid(),
      content,
      date: new Date(date).getTime(),
      emotion,
    },
  });
};

const onRemove = targetId => [dispatch({ type: 'REMOVE', targetId })];

const onEdit = (targetId, date, content, emotion) => {
  dispatch({
    type: 'EDIT',
    data: {
      id: targetId,
      content,
      date: new Date(date).getTime(),
      emotion,
    },
  });
};
```

### π Context μμ± λ° κ³΅κΈ

μμ£Ό μ¬μ©λ  κ² κ°μ μ μ²΄ μΌκΈ° λ°μ΄ν°μ νΉμ  νλμ νμν ν¨μλ€μ Contextλ‘ μμ±νμ¬ κ³΅κΈν©λλ€.<br />
`DiaryStateContext`λ₯Ό μμ±ν ν, Providerμ `value`λ‘ μνλ λ°μ΄ν°λ₯Ό κ°μ²΄ ννλ‘ λ¬Άμ΄μ μ λ¬ν΄μ€λλ€.

```javascript
// App.jsx

export const DiaryStateContext = createContext(); // μΌκΈ° λ°μ΄ν°
export const DiaryDispatchContext = createContext(); // λμ€ν¨μΉ ν¨μ

<DiaryStateContext.Provider value={diary}>
  <DiaryDispatchContext.Provider value={{ onCreate, onEdit, onRemove }}>
    { ... }
  </DiaryDispatchContext.Provider>
</DiaryStateContext.Provider>
```

### π μΌκΈ° λͺ©λ‘ λ μ§μ μ λ ¬

κΈ°μ€κ°μ΄ λλ `sortType`μ μ μν ν μ λ ¬ κΈ°μ€μ΄ μ΅μ μ(lastest)μΈμ§ μ€λλ μ(oldest)μΈμ§μ λ°λΌμ,<br />
`compare()` ν¨μλ₯Ό ν΅ν΄ 1μ°¨μ μΌλ‘ μ λ ¬ κ³Όμ μ κ±°μΉ νμ μλ‘μ΄ λ°°μ΄λ‘ λ³΅μ¬νμ¬ λ€μ μ΅μ’ μ λ ¬μ ν΄μ€λλ€.<br />
sortλ λ°°μ΄μ `sortedList` λΌλ λ³μμ ν λΉλλ©°, `getProcessedDiaryList()` ν¨μμ μ΅μ’ λ¦¬ν΄ κ°μ΄ λ©λλ€.<br />

```javascript
// DiaryList.jsx

const sortOptionList = [
  { value: 'latest', name: 'μ΅μ μ' },
  { value: 'oldest', name: 'μ€λλ μ' },
];

// Select Component
const ControlMenu = ({ value, onChange, optionList }) => {
  return (
    <select value={value} onChange={e => onChange(e.target.value)}>
      {optionList.map((item, index) => (
        <option key={index} value={item.value}>
          {item.name}
        </option>
      ))}
    </select>
  );
};

export default function DiaryList({ diaryList }) {
  const [sortType, setSortType] = useState('latest');

  const getProcessedDiaryList = () => {
    const compare = (a, b) => {
      if (sortType === 'latest') {
        return parseInt(b.date) - parseInt(a.date);
      } else {
        return parseInt(a.date) - parseInt(b.date);
      }
    };

    const copyList = JSON.parse(JSON.stringify(diaryList));
    const sortedList = copyList.sort(compare);
    return sortedList;
  };

  return (
    <div>
      <ControlMenu value={sortType} onChange={setSortType} optionList={sortOptionList} />
      {getProcessedDiaryList().map(diary => (
        <DiaryItem key={diary.id} {...diary} />
      ))}
    </div>
  );
}

DiaryList.defaultProps = {
  diaryList: [],
};
```

<br />

## π λ°°μ΄ μ  λ° λλ μ 

- λ¦¬μ‘νΈμμ μ΄λ€ μμΌλ‘ UIλ₯Ό μ»΄ν¬λνΈ λ¨μλ‘ μͺΌκ°μ΄μ νκΈ°ν΄ λκ° μ μλμ§ ν° νμ μ΄ν΄ν  μ μμμ΅λλ€.
- λͺ©λ‘ μ λ ¬ λ° μ λ¨μ λ μ§ κ³μ° λ± λλ¦ μ¬λ¬ κ°μ§ μλλ₯Ό ν΅ν΄ λ€μν κΈ°λ₯ κ΅¬νμ λν μμΌλ₯Ό λν μ μμμ΅λλ€.
- μ»΄ν¬λνΈ μ΅μ νμ λν μ΄ν΄λκ° λΆμ‘±νλ€κ³  λκΌκ³ , λΆμ‘±ν¨μ λλ λ§νΌ λμ± μ΄μ¬ν κ³΅λΆν΄μΌκ² λ€κ³  λ€μ§νμμ΅λλ€.
