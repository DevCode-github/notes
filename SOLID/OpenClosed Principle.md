It says that class should be open for extension but close for modification. In simpler terms it mean you should be able to add functionality to the class without changing its existing code.

### Example
#### Incorrect way
```
// Incorrect implementation violating OCP  
public class ReportGeneratorService {  
	public String generateReport(Report report) {  
		if ("PDF".equals(report.getReportType())) {  
			// Incorrect: Direct implementation for generating PDF report  
			return "PDF report generated";  
		} else if ("Excel".equals(report.getReportType())) {  
			// Incorrect: Direct implementation for generating Excel report  
			return "Excel report generated";  
		} else {  
			return "Unsupported report type";  
		}  
	}  
}
```

#### Correct way
```
public interface ReportGenerator {  
	String generateReport(Report report);  
}  
  
// Concrete implementation for generating PDF reports  
@Component  
public class PdfReportGenerator implements ReportGenerator {  
	@Override  
	public String generateReport(Report report) {  
		// Impl of pdf report  
		return String.format("PDF report generated for %s", report.getReportType());  
	}  
}  
  
// Concrete implementation for generating Excel reports  
@Component  
public class ExcelReportGenerator implements ReportGenerator {  
	@Override  
	public String generateReport(Report report) {  
		// Impl of excel report  
		return String.format("Excel report generated for %s", report.getReportType());  
	}  
}  
  
// Service that follows OCP  
@Service  
public class ReportGeneratorService {  
  
	private final Map<String, ReportGenerator> reportGenerators;  
	  
	@Autowired  
	public ReportGeneratorService(List<ReportGenerator> generators) {  
		// Initialize the map of report generators  
		this.reportGenerators = generators.stream()  
		.collect(Collectors.toMap(generator -> generator.getClass().getSimpleName(), Function.identity()));  
	}  
	  
	public String generateReport(Report report, String reportType) {  
		return reportGenerators.getOrDefault(reportType, unsupportedReportGenerator())  
		.generateReport(report);  
	}  
	  
	private ReportGenerator unsupportedReportGenerator() {  
		return report -> "Unsupported report type";  
	}  
}
```

**Interface ->**`ReportGenerator`

- Added **an interface (**`ReportGenerator`to define a common method for report generation.

**Concrete Implementations** ->`PdfReportGenerator` and `ExcelReportGenerator`

- Created classes implementing the interface **for PDF and Excel report generation**.
- Followed the Open-Closed principle of allowing extension without modifying existing code.

**Report Generator Service ->** `ReportGeneratorService`

- Introduced a service managing different report generator implementations.
- **Allows adding new report generators without changing existing code.**