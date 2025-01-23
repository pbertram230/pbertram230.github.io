ChatGPT 自推出以来，凭借其强大的自然语言处理能力，已经成为开发者的得力助手。尤其是在集成了相关插件的 VSCode 中，使用起来更加便捷。经过深度使用后，我发现 ChatGPT 在以下几个方面表现尤为出色：单元测试生成、可视化小工具开发、代码纠错、代码规范化以及编程辅助。接下来，我们一起来看看它的具体应用场景。

---

## 1. 单测用例生成

以下是一个静态工厂类的示例代码，用于根据传入的数据类型和语言类型创建多语言数据服务：

java
public class SepI18nDataFactory {

    public static ISepI18nDataService dataService;

    public static ISepI18nDataService buildDataService(SepI18nDataType dataType, SepI18nLanguageType languageType) {
        switch (dataType) {
            case DUCC:
                return getInstance(languageType, x -> new SepI18nDuccDataService(x));
            case MYSQL:
                return getInstance(languageType, x -> new SepI18nMysqlDataService(x));
            case CONFIG:
                return getInstance(languageType, x -> new SepI18nConfigDataService(x));
            default:
                return getInstance(languageType, x -> new SepI18nEmptyDataService(x));
        }
    }

    private static ISepI18nDataService getInstance(SepI18nLanguageType languageType, Function<SepI18nLanguageType, ISepI18nDataService> callback) {
        if (dataService == null) {
            synchronized (SepI18nDataFactory.class) {
                if (dataService == null) {
                    dataService = callback.apply(languageType);
                }
            }
        }
        return dataService;
    }
}


### ChatGPT 提问

**问题：为类中的所有方法创建单测并包含所有场景。**

### ChatGPT 生成的单测代码

java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class SepI18nDataFactoryTest {

    @Test
    public void testBuildDataService_DUCC() {
        SepI18nDataType dataType = SepI18nDataType.DUCC;
        SepI18nLanguageType languageType = SepI18nLanguageType.EN;
        ISepI18nDataService dataService = SepI18nDataFactory.buildDataService(dataType, languageType);
        assertEquals(SepI18nDuccDataService.class, dataService.getClass());
    }

    @Test
    public void testBuildDataService_MYSQL() {
        SepI18nDataType dataType = SepI18nDataType.MYSQL;
        SepI18nLanguageType languageType = SepI18nLanguageType.EN;
        ISepI18nDataService dataService = SepI18nDataFactory.buildDataService(dataType, languageType);
        assertEquals(SepI18nMysqlDataService.class, dataService.getClass());
    }

    @Test
    public void testBuildDataService_CONFIG() {
        SepI18nDataType dataType = SepI18nDataType.CONFIG;
        SepI18nLanguageType languageType = SepI18nLanguageType.EN;
        ISepI18nDataService dataService = SepI18nDataFactory.buildDataService(dataType, languageType);
        assertEquals(SepI18nConfigDataService.class, dataService.getClass());
    }

    @Test
    public void testBuildDataService_Default() {
        SepI18nDataType dataType = SepI18nDataType.OTHER;
        SepI18nLanguageType languageType = SepI18nLanguageType.EN;
        ISepI18nDataService dataService = SepI18nDataFactory.buildDataService(dataType, languageType);
        assertEquals(SepI18nEmptyDataService.class, dataService.getClass());
    }
}


生成的单测代码覆盖了所有场景，且能够完美运行。

---

## 2. 编写小工具

在日常工作中，我们经常需要开发一些小工具来辅助业务。以下是一个基于 JavaFX 的小工具示例，功能包括选择文件夹路径并显示其中的枚举类名。

### ChatGPT 提问

**问题：创建一个包含两个文本框和两个按钮的表单。点击第一个按钮选择文件夹路径并填入文本框，点击第二个按钮查找文件夹中的枚举类名并显示到另一个文本框中。**

### ChatGPT 生成的代码

java
import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.TextField;
import javafx.scene.layout.HBox;
import javafx.stage.DirectoryChooser;
import javafx.stage.Stage;

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.stream.Stream;

public class EnumFinder extends Application {

    private TextField directoryPathTextField;
    private TextField enumClassNameTextField;

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        directoryPathTextField = new TextField();
        Button directoryPathButton = new Button("Select Directory");
        directoryPathButton.setOnAction(e -> {
            DirectoryChooser directoryChooser = new DirectoryChooser();
            File selectedDirectory = directoryChooser.showDialog(primaryStage);
            if (selectedDirectory != null) {
                directoryPathTextField.setText(selectedDirectory.getAbsolutePath());
            }
        });

        Button findEnumButton = new Button("Find Enum");
        findEnumButton.setOnAction(e -> {
            String directoryPath = directoryPathTextField.getText();
            try (Stream<Path> paths = Files.walk(Paths.get(directoryPath))) {
                paths.filter(Files::isRegularFile)
                     .filter(p -> p.toString().endsWith(".class"))
                     .filter(this::isEnumClass)
                     .forEach(p -> enumClassNameTextField.setText(p.getFileName().toString().replace(".class", "")));
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        });

        enumClassNameTextField = new TextField();
        enumClassNameTextField.setEditable(false);

        HBox hBox = new HBox(directoryPathTextField, directoryPathButton, findEnumButton, enumClassNameTextField);
        Scene scene = new Scene(hBox);
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    private boolean isEnumClass(Path path) {
        try {
            Class<?> clazz = Class.forName(path.toString().replace(".class", ""));
            return clazz.isEnum();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
            return false;
        }
    }
}


稍作调整后，该工具即可正常运行。

---

## 3. 修复代码 Bug

### 示例 1：高并发 Bug

以下代码在高并发场景下可能会出现问题：

java
private static ISepI18nDataService getInstance(SepI18nLanguageType languageType, Function<SepI18nLanguageType, ISepI18nDataService> callback) {
    if (dataService == null) {
        dataService = callback.apply(languageType);
    }
    return dataService;
}


### ChatGPT 修复建议

问题在于 `dataService` 的同步控制不完善。修复后的代码如下：

java
private static ISepI18nDataService getInstance(SepI18nLanguageType languageType, Function<SepI18nLanguageType, ISepI18nDataService> callback) {
    if (dataService == null) {
        synchronized (SepI18nDataFactory.class) {
            if (dataService == null) {
                dataService = callback.apply(languageType);
            }
        }
    }
    return dataService;
}


---

### 示例 2：自定义锁 Bug

以下是一个基于 AQS 的自定义锁实现：

java
class SepSync extends AbstractQueuedSynchronizer {

    @Override
    protected boolean tryAcquire(int arg) {
        if (compareAndSetState(0, 1)) {
            setExclusiveOwnerThread(Thread.currentThread());
            return true;
        }
        return false;
    }

    @Override
    protected boolean tryRelease(int arg) {
        if (getState() == 0) {
            throw new IllegalMonitorStateException();
        }
        setExclusiveOwnerThread(null);
        setState(0);
        return true;
    }

    public void lock() {
        acquire(1);
    }

    public void unlock() {
        release(1);
    }
}


### ChatGPT 修复建议

问题在于 `tryRelease` 方法未检查当前线程是否为锁的持有者。修复后的代码如下：

java
@Override
protected boolean tryRelease(int arg) {
    if (Thread.currentThread() != getExclusiveOwnerThread()) {
        throw new IllegalMonitorStateException();
    }
    setExclusiveOwnerThread(null);
    setState(0);
    return true;
}


---

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bit.ly/bewildcard)