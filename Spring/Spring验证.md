#  Validation
   spring提供了一个Validator接口用来验证对象。他和Error对象一起使用，如果验证出现错误会向Error对象报告错误。
## 接口介绍
  1. supports(Class) 这个方法返回要验证的类
  2. validate(Object, org.springframework.validation.Errors) 验证给定对象，并把错误的情况放到Errors对象里。

  我们可以直接实现Validation接口做自定义操作，Spring也提供了ValidationUtils帮助类。
  ValidationUtils提供了很多方便的方法去验证对象中的某个字段是空或者是空字符串等等的情况下往Error对象中报告错误。
## 使用示例

    public class PersonValidator implements Validator {

        /**
         * This Validator validates *just* Person instances
         */
        public boolean supports(Class clazz) {
            return Person.class.equals(clazz);
        }

        public void validate(Object obj, Errors e) {
            ValidationUtils.rejectIfEmpty(e, "name", "name.empty");
            Person p = (Person) obj;
            if (p.getAge() < 0) {
                e.rejectValue("age", "negativevalue");
            } else if (p.getAge() > 110) {
                e.rejectValue("age", "too.darn.old");
            }
        }
    }

## 对嵌套类的验证
  比如我有一个Custom类，这个类中有一个嵌套类Address，如果想对Custom的字段进行验证的话，最好是Custom与Address分开验证。
  创建两个Validation对象并且进行嵌套。

        public class CustomerValidator implements Validator {

            private final Validator addressValidator;

            public CustomerValidator(Validator addressValidator) {
                if (addressValidator == null) {
                    throw new IllegalArgumentException("The supplied [Validator] is " +
                        "required and must not be null.");
                }
                if (!addressValidator.supports(Address.class)) {
                    throw new IllegalArgumentException("The supplied [Validator] must " +
                        "support the validation of [Address] instances.");
                }
                this.addressValidator = addressValidator;
            }

            /**
             * This Validator validates Customer instances, and any subclasses of Customer too
             */
            public boolean supports(Class clazz) {
                return Customer.class.isAssignableFrom(clazz);
            }

            public void validate(Object target, Errors errors) {
                ValidationUtils.rejectIfEmptyOrWhitespace(errors, "firstName", "field.required");
                ValidationUtils.rejectIfEmptyOrWhitespace(errors, "surname", "field.required");
                Customer customer = (Customer) target;
                try {
                    errors.pushNestedPath("address");
                    ValidationUtils.invokeValidator(this.addressValidator, customer.getAddress(), errors);
                } finally {
                    errors.popNestedPath();
                }
            }
        }

## 向Errors中注册错误消息
   ValidationUtils.rejectIfEmptyOrWhitespace(errors, "surname", "field.required");

   比如我们向errors中注册错误消息filed.required，spring不仅仅把错误消息注册进去还会注册
   1. filed.required.surname 一个是带有字段名字的错误消息
   2. filed.required.string  一个是带有字段类型的错误信息

   这样是为了方便开发者定位错误。

# Bean Validation
  bean validation是将业务领域模型与验证逻辑进行绑定。为验证java bean提供了很多元数据的支持。也就是注解。
  https://www.ibm.com/developerworks/cn/java/j-lo-jsr303/index.html

  例如@NotNull之类，可以放在变量上，接口上，get方法上等等。当验证出错的时候会将错误信息返回。

  而且我们可以自己开发constraint验证逻辑。一个constraint一般由一个注解annotation 和多个constraint validator组成，是一对多的关系。

## 自己实现一个验证
### 1. 写带有Constraint注解的annotation
   每个constraint应该有三个属性

   1. message 错误信息 key值默认使用类的全称加上.message
   2. group 验证组
   3. payLoad 一些扩展目的

### 2. validatedBy为annotation指定多个验证逻辑实现

### 3. 实现ConstraintValidator接口处理自己的验证逻辑