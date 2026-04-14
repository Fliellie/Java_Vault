```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Map;
import java.util.Scanner;
import java.util.stream.Collectors;

public class DocCauHinhNIO {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Nhập đường dẫn file config: ");
        String filePath = scanner.nextLine();

        try {
            // 1. Dùng Path để định vị file
            Path path = Paths.get(filePath);

            // 2. Đọc file, lọc dòng và đổ thẳng vào Map chỉ với 1 luồng xử lý (Stream)
            Map<String, String> configMap = Files.lines(path)
                .map(String::trim) // Xóa khoảng trắng 2 đầu mỗi dòng
                .filter(line -> !line.isEmpty() && line.contains("=")) // Bỏ qua dòng trống và dòng thiếu dấu '='
                .map(line -> line.split("=", 2)) // Tách làm 2 phần: key và value
                .collect(Collectors.toMap(
                    parts -> parts[0].trim(), // Key
                    parts -> parts[1].trim(), // Value
                    (existing, replacement) -> replacement // Nếu trùng key thì lấy cái sau
                ));

            // 3. Kiểm tra dữ liệu (Tận dụng lại hàm cũ)
            kiemTraDuLieu(configMap);
            
            System.out.println("Config loaded successfully.");

        } catch (java.nio.file.NoSuchFileException e) {
            // NIO ném ra lỗi rất cụ thể khi không tìm thấy file thay vì FileNotFoundException
            System.out.println("Config file not found.");
        } catch (IOException e) {
            System.out.println("I/O error.");
            e.printStackTrace();
        } catch (NumberFormatException e) {
            System.out.println("Invalid number format.");
        } catch (InvalidConfigException e) {
            System.out.println("Invalid config: " + e.getMessage());
        } finally {
            // 4. Không cần reader.close() nữa! Files.lines() tự đóng file cực kỳ an toàn.
            System.out.println("Program finished.");
            scanner.close();
        }
    }

    // Giữ nguyên hàm kiểm tra logic từ bài trước
    private static void kiemTraDuLieu(Map<String, String> map) throws InvalidConfigException {
        if (!map.containsKey("username")) throw new InvalidConfigException("Missing username");
        if (!map.containsKey("timeout")) throw new InvalidConfigException("Missing timeout");

        int timeout = Integer.parseInt(map.get("timeout"));
        if (timeout <= 0) throw new InvalidConfigException("timeout must be > 0");

        if (map.containsKey("maxConnections")) {
            int maxConn = Integer.parseInt(map.get("maxConnections"));
            if (maxConn < 1) throw new InvalidConfigException("maxConnections must be >= 1");
        }
    }
}

// Giữ nguyên Custom Exception
class InvalidConfigException extends Exception {
    public InvalidConfigException(String message) { super(message); }
}
```