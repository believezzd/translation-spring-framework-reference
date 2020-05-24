##### Lookup method injection

和前面注意提示一样，lookup方法注入是一个你很难使用到的高级特性。当单例bean需要原型bean时这个方法很有用。使用Java为这种类型的配置提供了一种自然的方式用于实现这种模式。

```
public abstract class CommandManager {
    public Object process(Object commandState) {
    // grab a new instance of the appropriate Command interface
        Command command = createCommand();
        // set the state on the (hopefully brand new) Command instance
        command.setState(commandState);
        return command.execute();
    }
    // okay... but where is the implementation of this method?
    protected abstract Command createCommand();
}
```

适应java配置支持，你可以创建一个CommandManager的子类重写createCommand方法以这样的方式使得看起来是一个新的（原型的）command object。

```
@Bean
@Scope("prototype")
public AsyncCommand asyncCommand() {
    AsyncCommand command = new AsyncCommand();
    // inject dependencies here as required
        return command;
    }
    @Bean
    public CommandManager commandManager() {
        // return new anonymous implementation of CommandManager with command() overridden
        // to return a new prototype Command object
        return new CommandManager() {
            protected Command createCommand() {
                return asyncCommand();
        }
    }
}
```