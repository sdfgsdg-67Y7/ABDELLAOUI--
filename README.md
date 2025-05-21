# ABDELLAOUI-éducation  
import React, { useState } from 'react';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Select, SelectTrigger, SelectValue, SelectContent, SelectItem } from '@/components/ui/select';

const questions = {
  'الانجليزية': Array.from({ length: 100 }, (_, i) => ({
    question: `سؤال ${i + 1} في الانجليزية`,
    options: ['إجابة 1', 'إجابة 2', 'إجابة 3', 'إجابة صحيحة'],
    answer: 'إجابة صحيحة'
  })),
  'الفرنسية': Array.from({ length: 100 }, (_, i) => ({
    question: `سؤال ${i + 1} في الفرنسية`,
    options: ['Réponse 1', 'Réponse 2', 'Réponse correcte', 'Réponse 4'],
    answer: 'Réponse correcte'
  })),
  'العلوم': Array.from({ length: 100 }, (_, i) => ({
    question: `سؤال ${i + 1} في العلوم`,
    options: ['خيار 1', 'خيار 2', 'خيار صحيح', 'خيار 4'],
    answer: 'خيار صحيح'
  })),
  'الفيزياء': Array.from({ length: 100 }, (_, i) => ({
    question: `سؤال ${i + 1} في الفيزياء`,
    options: ['اختيار 1', 'اختيار 2', 'إجابة صحيحة', 'اختيار 4'],
    answer: 'إجابة صحيحة'
  })),
  'التربية الاسلامية': Array.from({ length: 100 }, (_, i) => ({
    question: `سؤال ${i + 1} في التربية الاسلامية`,
    options: ['خيار 1', 'الإجابة الصحيحة', 'خيار 3', 'خيار 4'],
    answer: 'الإجابة الصحيحة'
  })),
  'الرياضيات': Array.from({ length: 100 }, (_, i) => ({
    question: `سؤال ${i + 1} في الرياضيات`,
    options: ['1', '2', '3', '4'],
    answer: '3'
  })),
};

export default function QuizApp() {
  const [selectedSubject, setSelectedSubject] = useState('');
  const [currentQuestions, setCurrentQuestions] = useState([]);
  const [userAnswers, setUserAnswers] = useState({});
  const [score, setScore] = useState(null);

  const handleSelect = (value) => {
    setSelectedSubject(value);
    setCurrentQuestions(questions[value]);
    setUserAnswers({});
    setScore(null);
  };

  const handleAnswer = (index, answer) => {
    setUserAnswers((prev) => ({ ...prev, [index]: answer }));
  };

  const handleSubmit = () => {
    const correctCount = currentQuestions.reduce((acc, q, i) => {
      return acc + (userAnswers[i] === q.answer ? 1 : 0);
    }, 0);
    setScore(correctCount);
  };

  return (
    <div className="p-6 max-w-4xl mx-auto">
      <h1 className="text-2xl font-bold mb-4 text-center">اختبر معلوماتك حسب المادة</h1>
      <Select onValueChange={handleSelect}>
        <SelectTrigger className="w-full mb-4">
          <SelectValue placeholder="اختر المادة" />
        </SelectTrigger>
        <SelectContent>
          {Object.keys(questions).map((subject) => (
            <SelectItem key={subject} value={subject}>
              {subject}
            </SelectItem>
          ))}
        </SelectContent>
      </Select>

      {selectedSubject && (
        <>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mt-6">
            {currentQuestions.slice(0, 10).map((q, index) => (
              <Card key={index} className="shadow-xl">
                <CardContent className="p-4 text-right">
                  <p className="font-semibold mb-2">{q.question}</p>
                  {q.options.map((opt, i) => (
                    <Button
                      key={i}
                      variant={userAnswers[index] === opt ? 'default' : 'outline'}
                      onClick={() => handleAnswer(index, opt)}
                      className="w-full mb-2"
                    >
                      {opt}
                    </Button>
                  ))}
                </CardContent>
              </Card>
            ))}
          </div>
          <div className="text-center mt-6">
            <Button onClick={handleSubmit}>تأكيد الإجابات</Button>
            {score !== null && <p className="mt-4 text-xl">النتيجة: {score} من 10</p>}
          </div>
        </>
      )}
    </div>
  );
}
