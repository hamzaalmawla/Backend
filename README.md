# Library Management System - Backend API

Flask-based REST API for Library Management System.

## Features

- ✅ User Authentication (JWT)
- ✅ Book Management e:
        print(f"Error sending email: {str(e)}")
        return False


def send_overdue_notification(loan):
    """
    Send email notification for overdue book
    
    Args:
        loan: Loan object to send notification for
    """
    from flask_mail import Message
    from app import mail
    
    try:
        subject = "Overdue Book Notification - Library Management System"
        body = f"""
        Dear {loan.user.name},
        
        The following book is overdue:
        
        Book: {loan.book.title}
        Author: {loan.book.author}
        Due Date: {loan.due_date.strftime('%Y-%m-%d')}
        Days Overdue: {(datetime.utcnow() - loan.due_date).days}
        Fine Amount: ${loan.fine_amount:.2f}
        
        Please return the book as soon as possible.
        
        Thank you,
        Library Management System
        """
        
        msg = Message(subject=subject, recipients=[loan.user.email], body=body)
        mail.send(msg)
        return True
    except Exception as e:
        print(f"Error sending email: {str(e)}")
        return False


def validate_isbn(isbn):
    """
    Validate ISBN format (13 digits)
    
    Args:
        isbn: ISBN string to validate
        
    Returns:
        bool: True if valid, False otherwise
    """
    if not isbn:
        return False
    
    # Remove any hyphens or spaces
    isbn = isbn.replace('-', '').replace(' ', '')
    
    # Check if 13 digits
    if len(isbn) != 13 or not isbn.isdigit():
        return False
    
    return True


def validate_email(email):
    """
    Validate email format
    
    Args:
        email: Email string to validate
        
    Returns:
        bool: True if valid, False otherwise
    """
    import re
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None