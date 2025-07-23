```
use mydb;

-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `mydb` DEFAULT CHARACTER SET utf8 ;
USE `mydb` ;

-- -----------------------------------------------------
-- Table `mydb`.`dept`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`dept` (
  `dept_id` INT NOT NULL,
  `dept_name` VARCHAR(45) NULL,
  PRIMARY KEY (`dept_id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`emp`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`emp` (
  `emp_id` INT NOT NULL,
  `emp_name` VARCHAR(45) NULL,
  `position` VARCHAR(30) NULL,
  `dept_id` INT NULL,
  `dept_dept_id` INT NOT NULL,
  PRIMARY KEY (`emp_id`),
  INDEX `fk_emp_dept1_idx` (`dept_dept_id` ASC) VISIBLE,
  CONSTRAINT `fk_emp_dept1`
    FOREIGN KEY (`dept_dept_id`)
    REFERENCES `mydb`.`dept` (`dept_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`dependent`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`dependent` (
  `emp_id` INT NULL,
  `seq` INT NOT NULL,
  `name` VARCHAR(45) NULL,
  `relationship` VARCHAR(45) NULL,
  `emp_emp_id` INT NOT NULL,
  PRIMARY KEY (`seq`),
  INDEX `fk_dependent_emp1_idx` (`emp_emp_id` ASC) VISIBLE,
  CONSTRAINT `fk_dependent_emp1`
    FOREIGN KEY (`emp_emp_id`)
    REFERENCES `mydb`.`emp` (`emp_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`work`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`work` (
  `emp_id` INT NULL,
  `dept_id` INT NULL,
  `seq` INT NOT NULL,
  `period` VARCHAR(45) NULL,
  `position` VARCHAR(45) NULL,
  `dependent_seq` INT NOT NULL,
  `emp_emp_id` INT NOT NULL,
  PRIMARY KEY (`seq`),
  INDEX `fk_work_dependent1_idx` (`dependent_seq` ASC) VISIBLE,
  INDEX `fk_work_emp1_idx` (`emp_emp_id` ASC) VISIBLE,
  CONSTRAINT `fk_work_dependent1`
    FOREIGN KEY (`dependent_seq`)
    REFERENCES `mydb`.`dependent` (`seq`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_work_emp1`
    FOREIGN KEY (`emp_emp_id`)
    REFERENCES `mydb`.`emp` (`emp_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

```



```
use mydb;

-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `mydb` DEFAULT CHARACTER SET utf8 ;
USE `mydb` ;

-- -----------------------------------------------------
-- Table `mydb`.`parking`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`parking` (
  `parking_lot_name` VARCHAR(50) NOT NULL,
  `location` VARCHAR(45) NULL,
  `capacity` INT NULL,
  `floor` INT NULL,
  PRIMARY KEY (`parking_lot_name`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`emp`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`emp` (
  `emp_id` INT NOT NULL,
  `emp_name` VARCHAR(45) NULL,
  `phone` VARCHAR(45) NULL,
  `license_no` VARCHAR(45) NULL,
  PRIMARY KEY (`emp_id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb`.`column`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb`.`column` (
  `space_seq` INT NOT NULL,
  `parking_lot_name` VARCHAR(45) NULL,
  `emp_id` INT NULL,
  `emp_emp_id` INT NOT NULL,
  `parking_parking_lot_name` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`space_seq`),
  INDEX `fk_column_emp_idx` (`emp_emp_id` ASC) VISIBLE,
  INDEX `fk_column_parking1_idx` (`parking_parking_lot_name` ASC) VISIBLE,
  CONSTRAINT `fk_column_emp`
    FOREIGN KEY (`emp_emp_id`)
    REFERENCES `mydb`.`emp` (`emp_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_column_parking1`
    FOREIGN KEY (`parking_parking_lot_name`)
    REFERENCES `mydb`.`parking` (`parking_lot_name`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
```
