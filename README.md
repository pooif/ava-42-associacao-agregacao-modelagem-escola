# 4.2 // Associação, Agregação e Modelagem // Escola

Use este link do GitHub Classroom para ter sua cópia alterável deste repositório: <>

Implementar respeitando os fundamentos de Orientação a Objetos.

**Tópicos desta atividade:** associações, dependências, agregações, modelagem

---

Implementar sistema para cursos FIC e seus relacionamentos. Considere um sistema escolar, implementado em parte. A escola oferece cursos de curta duração (Formação Inicial e Continuada - FIC). Cada curso tem uma carga horária determinada pela soma da carga horária de seus componentes curriculares. Quando um curso é oferecido ele forma uma turma, que possui quantidade limitada de vagas, data de início e fim. Os alunos podem se matricular no curso e cancelar a matrícula, desde que o curso esteja em andamento. O curso pode começar ou terminar em data diferente da prevista, neste caso essas datas são redefinidas. Os cursos podem ser cancelados, mas somente antes de começar. Quando o curso é concluído, todos os estudantes ativos (que não cancelaram sua matrícula) são concluintes.

Casos de teste:

```java
Curso infbas = new Curso("Informatica Basica");

Componente c1 = new Componente("Windows", 20); // Windows, 20h
Componente c2 = new Componente("Word", 30); // Windows, 30h
Componente c3 = new Componente("Excel", 30); // Windows, 30h

infbas.addComponente(c1);
infbas.addComponente(c2);
infbas.addComponente(c3);

System.out.println(c1.getNome().equals("Windows"));
System.out.println(c2.getNome().equals("Word"));
System.out.println(c3.getNome().equals("Excel"));

System.out.println(c1.getCargaHoraria() == 20);
System.out.println(c2.getCargaHoraria() == 30);
System.out.println(c3.getCargaHoraria() == 30);

System.out.println(infbas.getNome().equals("Informatica Basica"));
System.out.println(infbas.getCargaHoraria() == 80);

System.out.println(c1.toString().equals("Windows 20h"));
System.out.println(c2.toString().equals("Word 30h"));
System.out.println(c3.toString().equals("Excel 30h"));

c1.setCargaHoraria(30);

// os curso mudam de carga horária se os componentes mudarem
System.out.println(c1.getCargaHoraria() == 30);
System.out.println(infbas.getCargaHoraria() == 90);

// datas de início e término no padrão ISO: yyyy-mm-dd
Turma t1 = new Turma(infbas, "2016-11-02", "2016-12-15", 3); // 3 vagas

// as turmas obtém um instantâneo do curso,

c1.setCargaHoraria(100);
System.out.println(c1.getCargaHoraria() == 100);
System.out.println(infbas.getCargaHoraria() == 160);

// se o curso (ou componente) mudar de carga horária ou estrutura
// as turmas não mudam! Elas têm um instantâneo do curso quando
// são criadas.
System.out.println(t1.getCurso().getComponenteCurricular(1) == 30);
System.out.println(t1.getCurso().getCargaHoraria() == 90);

System.out.println(t1.getCargaHoraria() == 90); // mesma do curso

System.out.println(t1.getVagas() == 3);
System.out.println(t1.getCurso().equals(infbas));

// até aqui 0.5 ponto -------------------------

Aluno a1 = new Aluno("Paulo", "88833344422");
Aluno a2 = new Aluno("Pedro", "99933344422");
Aluno a3 = new Aluno("Paola", "00033344422");
Aluno a4 = new Aluno("Pamela", "11133344422");

System.out.println(t1.temVagas() == true);

System.out.println(t1.getQuantidadeAlunos() == 0);

t1.matricular(a1);
t1.matricular(a2);
t1.matricular(a3);

System.out.println(t1.getQuantidadeAlunos() == 3);

System.out.println(t1.temVagas() == false);

// t1.matricular(a4); // deve lançar uma exceção (máximo 3 vagas)

System.out.println(t1.isNova() == true);
System.out.println(t1.isAndamento() == false);
System.out.println(t1.isCancelada() == false);
System.out.println(t1.isConcluida() == false);

t1.comecar();

System.out.println(t1.isNova() == false);
System.out.println(t1.isAndamento() == true);
System.out.println(t1.isCancelada() == false);
System.out.println(t1.isConcluida() == false);

System.out.println(t1.getQuantidadeAlunosAtivos() == 3);

t1.cancelarMatricula(a1);

System.out.println(t1.getQuantidadeAlunos() == 3);
System.out.println(t1.getQuantidadeAlunosAtivos() == 2);

// t1.cancelar(); // deve lançar exceção, não pode cancelar turma em andamento

t1.concluir();

System.out.println(t1.isNova() == false);
System.out.println(t1.isAndamento() == false);
System.out.println(t1.isCancelada() == false);
System.out.println(t1.isConcluida() == true);

// t1.cancelarMatricula(a2); // deve lançar exceção, não pode cancelar matrícula se turma concluída

Curso q = new Curso("Outro curso");
q.addComponente(new Componente("Outro componente", 10));

Turma t2 = new Turma(q, "2017-01-12", "2017-03-09", 3);

System.out.println(t2.isNova() == true);
System.out.println(t2.isAndamento() == false);
System.out.println(t2.isCancelada() == false);
System.out.println(t2.isConcluida() == false);

t2.matricular(new Aluno("aaa", "22233300011"));

t2.cancelar(); // pode cancelar

System.out.println(t2.isNova() == false);
System.out.println(t2.isAndamento() == false);
System.out.println(t2.isCancelada() == true);
System.out.println(t2.isConcluida() == false);

// t2.concluir(); // deve lançar exceção, turma cancelada não pode ser concluida

Matricula m1 = t1.getAluno(a1);
System.out.println(m1.isCancelada() == true);

Matricula m2 = t1.getAluno(a2);
System.out.println(m2.isCancelada() == false);
System.out.println(m2.isConcluida() == true);

// a turma 2 foi cancelada então a matricula do aluno também foi
Matricula m3 = t2.getAluno(1);
System.out.println(m3.isCancelada() == true);

Matricula m4 = t2.getAluno(2); // não há aluno 2
System.out.println(m4 == null);
```


---
Obs.: os casos de teste não podem ser alterados, mas outros podem ser adicionados. Fique a vontade para adicionar códigos que imprimem ou separam os testes, por exemplo.
