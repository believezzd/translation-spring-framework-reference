#### @Componet and further stereotype annotation

@Repository注解用于描述存储的角色（常见的就是DAO）。在所有使用这个注解的功能中有一个是自动异常转换，见20.2.2节“异常translation”

spinrg提供了很多模板注解：@Component、@Service和@Controller。@Component是用于spring管理组件的泛型模板。@Repository、@Service和@Controller是@Component的特例用于更清晰的定义，例如，在持久化、服务和表现层。你可以将你的组件类都声明为@Component，但是通过@Repository、@Service和@Controller会更加的方便出来并与切面相互结合。例如，这些策略可以成为目标的切入点。而且这些在spirng框架的后续版本中会有特殊的功能。如果你在服务层纠结使用@Component还是@Service，那很明显@Service是更好的选择。同样，在开始的时候，@Repository支持持久层的异常translation。